# @tiptaptop/extension-image-paste

Hi! This is an extension for [tiptap](https://github.com/ueberdosis/tiptap). It adds a listener that listens for paste and drop events, and in the case that what is pasted/dropped is an image, will pass the file out to the editor instance to be handled from your app (in case you want to upload it). It also allows preventing pasting/uploading in case you need that.

## By default, it uses a `fileMatchRegex` option value that accepts gif/jpeg/png/svg/webp/bmp. `disableImagePaste` is the option to disable paste, in which case, we just return the image's url.

## On the editor-side, you can provide `onImagePaste()`, `onImageDrop()` and `onDisabledImagePaste()` callbacks, and respond in context to what's happening.

This isn't a perfect solution, as the extension isn't asynchronous, and can't wait for the callbacks to return a value. Instead, you'll have to add the image-url/markup on the editor-side, through the normal mechanism, eg, `editor.chain().focus().setImage({ src: url }).run();`

## Usage

I'm primarily sharing this repo to get some feedback, but I will eventually publish an installable version on npm. Until then, you can just copy and paste the image-paste.ts file into your own project, and import it wherever you use a tiptap editor. For example:

```ts
import { ImagePaste } from '...path...to/image-paste';
...
const editor  = new Editor({
      extensions: [
          Bold,
          ImagePaste.configure({
            fileMatchRegex: /^image\/(gif|jpe?g|a?png|svg|webp|bmp)/i,
            disableImagePaste: false,
            render: () => {
              return {
                onImagePaste: files => {
                  files.forEach(file => {
                    // upload image here, then add to the editor eg, editor.chain().focus().setImage({ src: url }).run();
                  });
                },
                onDisabledImagePaste: text => {
                  // add text to editor if you want, or display an error/upselll message
                },
                onImageDrop: files => {
                  files.forEach(file => {
                    // same as paste, upload the image and send it to the editor
                  });
                },
              };
            },
          }),
      ],
});
```
