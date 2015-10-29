# Uploadable

Uploadable behavior provides the tools to manage the persistence of files with Doctrine 2, including automatic handling of moving, renaming and removal of files and other features.

- Extension moves, removes and renames files according to configuration automatically
- Lots of options: Allow overwrite, append a number if file exists, filename generators, post-move callbacks, etc.
- It can be extended to work not only with uploaded files, but with files coming from any source (an URL, another
 file in the same server, etc).
- Validation of size and mime type

### Class annotation

> @Gedmo\Mapping\Annotation\Uploadable

This class annotation tells if a class is Uploadable.

| Annotations | Description |
|--|--|
|**allowOverwrite** | If this option is true, it will overwrite a file if it already exists. If you set "false", an exception will be thrown. Default: false|
|**appendNumber** | If this option is true and "allowOverwrite" is false, in the case that the file already exists, it will append a number to the filename. Example: if you're uploading a file named "test.txt", if the file already exists and this option is true, the extension will modify the name of the uploaded file to "test1.txt", where "1" could be any number. The extension will check if the file exists until it finds a filename with a number as its postfix that is not used. If you use a filename generator and this option is true, it will append a number to the filename anyway if a file with the same name already exists. Default value: false|
|**path** | This option expects a string containing the path where the files represented by this entity will be moved. Default: "". Path can be set in other ways: From the listener or from a method. More details later.|
|**pathMethod** | Similar to option "path", but this time it represents the name of a method on the entity that will return the path to which the files represented by this entity will be moved. This is useful in several cases. For example, you can set specific paths for specific entities, or you can get the path from other sources (like a framework configuration) instead of hardcoding it in the entity. Default: "". As first argument this method takes default path, so you can return path relative to default.|
|**callback** | This option allows you to set a method name. If this option is set, the method will be called after the file is moved. Default value: "". As first argument, this method can receive an array with information about the uploaded file, which includes the following keys: **fileName:**, **fileExtension**, **fileWithoutExt**, **filePath**, **fileMimeType**, **fileSize** .|
|**filenameGenerator**| This option allows you to set a filename generator for the file. There are two already included by the extension: SHA1, which generates a sha1 filename for the file, and ALPHANUMERIC, which "normalizes" the filename, leaving only alphanumeric characters in the filename, and replacing anything else with a "-". You can even create your own FilenameGenerator class (implementing the Gedmo\Uploadable\FilenameGenerator\FilenameGeneratorInterface) and set this option with the fully qualified class name. The other option available is "NONE" which, as you may guess, means no generation for the filename will occur. Default: "NONE".|
|**maxSize**| This option allows you to set a maximum size for the file in bytes. If file size exceeds the value set in this configuration, an exception of type "UploadableMaxSizeException" will be thrown. By default, its value is set to 0, meaning that no size validation will occur.|
|**allowedTypes**| With this option you can set a comma-separated list of allowed mime types for the file. The extension will use a simple mime type guesser to guess the file type, and then it will compare it to the list of allowed types. If the mime type is not valid, then an exception of type "UploadableInvalidMimeTypeException" will be thrown. If you set this option, you can't set the disallowedTypes option described next. By default, no validation of mime type occurs. If you want to use a custom mime type guesser, see this.|
|**disallowedTypes**| Similar to the option allowedTypes, but with this one you configure a "black list" of mime types. If the mime type of the file is on this list, n exception of type "UploadableInvalidMimeTypeException" will be thrown. If you set this option, you can't set the allowedTypes option described next. By default, no validation of mime type occurs. If you want to use a custom mime type guesser, see this.|

### Property annotation

> @Gedmo\Mapping\Annotation\UploadableFilePath

This annotation is used to set which field will receive the path to the file. The field MUST be of type "string". Either this one or UploadableFileName annotation is REQUIRED to be set.

> @Gedmo\Mapping\Annotation\UploadableFileName

This annotation is used to set which field will receive the name of the file. The field MUST be of type "string". Either this one or UploadableFilePath annotation is REQUIRED to be set.

> @Gedmo\Mapping\Annotation\UploadableFileMimeType

This is an optional annotation used to set which field will receive the mime type of the file as its value. This field MUST be of type "string".

> @Gedmo\Mapping\Annotation\UploadableFileSize

This is an optional annotation used to set which field will receive the size in bytes of the file as its value. This field MUST be of type "decimal".


```php
<?php
use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @Gedmo\Uploadable(path="your/path")
 */
class File
{
    /**
     * @ORM\Column(name="path", type="string")
     * @Gedmo\UploadableFilePath
     */
    protected $path;
}
``` 
### Usage
In order to use this extension, you need to add the `LaravelDoctrine\Extensions\Uploadable\UploadableExtensionServiceProvider` into your `providers` array in the Laravel's `app.php` config. This service provider will register a singleton for the `UploadableListener` to be used in your app.

To get the listener singleton you can then use its alias `LaravelDoctrine\Extensions\Uploadable\UploadableListener::class`.

Example with the `app()` shortcut:
```
$listener = app(UploadableListener::class);
$listener->addEntityFileInto($entity, $fileInfo);
```

Another way would be to also register the `LaravelDoctrine\Extensions\Uploadable\UploadableFacade::class` facade in the `aliases` array in the Laravel's `app.php`. Then you can use the facade to call the listener's methods.

Example with a facade (named `Uploadable`):
```
\Uploadable::addEntityFileInfo($entity, $fileInfo);
```


For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/uploadable.md)
