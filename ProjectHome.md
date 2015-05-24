This project strives to create a fast and efficient PHP Mime Mail Parser Class using PHP's [MailParse](http://pecl.php.net/package/mailparse) Extension.

Many have branched this project and fixed many issues. Take a look at the issues list for branches and fixes.

We've also built a full email hosting from what we've learned here. [Fiji Cloud Email](http://www.fijisoftware.com/)


## Requirements and Installation ##
The Mbstring extension must be installed.
The MailParse PECL extension must be installed.
See the docs: https://code.google.com/p/php-mime-mail-parser/w/list

## Example Usage ##

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

  1. specify a file path to the mime mail.
  1. specify a php file resource (stream) to the mime mail
  1. specify the raw mime mail text

These are done with:

  1. $Parser->setPath($path);
  1. $Parser->setStream(fopen($path));
  1. $Parser->setText(file\_read\_contents($path));

respectively.

You only need to set one however. The preferred would be either setting the path, or the stream resource.

The only streams you can use are STDIN and file resources. Any stream that has requires a specific protocol, such as IMAP or POP, is NOT supported. You will need to download the email to a file first and then pass it to MimeMailParser.

Setting a path or stream ensures that the mime mail is parsed in increments and does not require a lot of memory.

When retrieving attachments, it returns an array of MimeMailParser\_attachment class instances:

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

The above code writes each attachment to the directory $save\_dir. The method $attachment->read() will read a length of bytes from the attachment until the end of the file. That way you can read large attachments without consuming large amounts of memory.

Here is an example of reading an email from STDIN: http://www.bucabay.com/2009/web-development/incoming-mail-php-mime-mail-parser/

## Notes ##

This class is just a wrapper around PHP's [MailParse](http://pecl.php.net/package/mailparse) extension. It is not intended for redistributable code. It is an attempt to provide a simple, efficient and fast parser.

There are many pure PHP implementations of mime mail parsing, but those are generally slow.

## Download and Sources ##

There are no stable packages to download yet. Just grab the latest source from the [repository](http://code.google.com/p/php-mime-mail-parser/source/browse/trunk/MimeMailParser.class.php).

## Documentation ##

You can find some [quick docs here](http://code.google.com/p/php-mime-mail-parser/wiki/RequirementsAndInstallation).