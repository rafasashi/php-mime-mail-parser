# php-mime-mail-parser

This project strives to create a fast and efficient PHP Mime Mail Parser Class using PHP's MailParse Extension.

**Requirements and Installation**

The Mbstring extension must be installed. The MailParse PECL extension must be installed. See the docs: https://code.google.com/p/php-mime-mail-parser/w/list

Example Usage

 ```

  <?php
  
  require_once('MimeMailParser.class.php');
  
  $path = 'path/to/mail.txt';
  $Parser = new MimeMailParser();
  $Parser->setPath($path);
  
  $to = $Parser->getHeader('to');
  $from = $Parser->getHeader('from');
  $subject = $Parser->getHeader('subject');
  $text = $Parser->getMessageBody('text');
  $html = $Parser->getMessageBody('html');
  $attachments = $Parser->getAttachments();
  
  ?>
  
 ```

There are three input methods of the mime mail to be parsed.

specify a file path to the mime mail.
specify a php file resource (stream) to the mime mail
specify the raw mime mail text

These are done with:

 ```
  $Parser->setPath($path);
  $Parser->setStream(fopen($path));
  $Parser->setText(file_read_contents($path));
 ```
 
respectively.

You only need to set one however. The preferred would be either setting the path, or the stream resource.

The only streams you can use are STDIN and file resources. Any stream that has requires a specific protocol, such as IMAP or POP, is NOT supported. You will need to download the email to a file first and then pass it to MimeMailParser.

Setting a path or stream ensures that the mime mail is parsed in increments and does not require a lot of memory.

When retrieving attachments, it returns an array of MimeMailParser_attachment class instances:

 ```
  $attachments = $Parser->getAttachments();
 ```
 
You can then iterate through the array:

 ```
  $save_dir = '/path/to/save/attachments/';
  foreach($attachments as $attachment) {
    // get the attachment name
    $filename = $attachment->filename;
    // write the file to the directory you want to save it in
    if ($fp = fopen($save_dir.$filename, 'w')) {
      while($bytes = $attachment->read()) {
        fwrite($fp, $bytes);
      }
      fclose($fp);
    }
  }
 ```
The above code writes each attachment to the directory  `$save_dir `. The method  `$attachment->read() ` will read a length of bytes from the attachment until the end of the file. That way you can read large attachments without consuming large amounts of memory.

Notes
This class is just a wrapper around PHP's MailParse extension. It is not intended for redistributable code. It is an attempt to provide a simple, efficient and fast parser.

There are many pure PHP implementations of mime mail parsing, but those are generally slow.
