Example code     {#cordova-examples}
===================================

For barcode scanner usage examples, you can either use one of the sample apps included in the plugin archive, or paste one of the samples below into your www/index.html file.

## Build the sample apps

In order to build the sample apps you must import one of them upon creation of your project as follows.

Note that you will still need to add the platform and plugin as described in {@link cordova-integrate here}. You will also need to replace the license key in the samples with your license key.

### Simple sample

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.java}
> cordova create helloworld --id "com.scandit.helloworld" --template <path-to-repository>/samples/simple/www/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Extended sample

For the extended sample, the license key is in `<path-to-repository>/samples/extended/src/providers/scanner.ts`, in the `setAppKey` method. After setting the license key, running `npm run build` is necessary for the build files in the `www` folder to be updated — the recommendation is to add the license key before running the commands below.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.java}
> cd <path-to-repository>/samples/extended
> npm install
> npm run build
> cd <path-to-folder-to-contain-cordova-project>
> cordova create helloworld --id "com.scandit.helloworld" --link-to <path-to-repository>/samples/extended/www
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Note that the `--link-to` option will symlink to the specified www directory without creating a copy. For more information, see https://cordova.apache.org/docs/en/latest/reference/cordova-cli/#cordova-create-command.

### MatrixScan sample

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.java}
> cordova create helloworld --id "com.scandit.helloworld" --template <path-to-repository>/samples/matrixscan/www/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### OCR sample

For the OCR sample, the license key is in `<path-to-repository>/samples/ocr/src/providers/scanner.ts`, in the `setAppKey` method. After setting the license key, running `npm run build` is necessary for the build files in the `www` folder to be updated — the recommendation is to add the license key before running the commands below.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.java}
> cd <path-to-repository>/samples/ocr
> npm install
> npm run build
> cd <path-to-folder-to-contain-cordova-project>
> cordova create helloworld --id "com.scandit.helloworld" --link-to <path-to-repository>/samples/ocr/www
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Note that the `--link-to` option will symlink to the specified www directory without creating a copy. For more information, see https://cordova.apache.org/docs/en/latest/reference/cordova-cli/#cordova-create-command.

## Simple Fullscreen

This shows the simplest way of using the plugin. The scanner is opened full screen and is closed as soon as a barcode is scanned, returning the result to a function specified by you.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.java}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="format-detection" content="telephone=no" />
        <meta name="msapplication-tap-highlight" content="no" />
        <meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1,
            minimum-scale=1, width=device-width, height=device-height" />
        <title>Scandit Barcode Scanner</title>
    </head>
    <body style='background:black'>
        <script type="text/javascript" src="cordova.js"></script>
        <script type="text/javascript">
            // On Windows, the alert function doesn't exist, so we add it.
            window.alert = window.alert !== undefined ? window.alert : function (message) {
                var alertBox = new Windows.UI.Popups.MessageDialog(message);
                alertBox.showAsync();
            };
            function success(session) {
                alert("Scanned " + session.newlyRecognizedCodes[0].symbology
                        + " code: " + session.newlyRecognizedCodes[0].data);
            }
            function failure(error) {
                alert("Failed: " + error);
            }
            function scan() {
                Scandit.License.setAppKey("-- ENTER YOUR LICENSE KEY HERE --");
                var settings = new Scandit.ScanSettings();
                settings.setSymbologyEnabled(Scandit.Barcode.Symbology.EAN13, true);
                settings.setSymbologyEnabled(Scandit.Barcode.Symbology.UPC12, true);
                settings.setSymbologyEnabled(Scandit.Barcode.Symbology.EAN8, true);
                settings.setSymbologyEnabled(Scandit.Barcode.Symbology.CODE39, true);

                // Some 1d barcode symbologies allow you to encode variable-length data. By default, the
                // Scandit BarcodeScanner SDK only scans barcodes in a certain length range. If your
                // application requires scanning of one of these symbologies, and the length is falling
                // outside the default range, you may need to adjust the "active symbol counts" for this
                // symbology. This is shown in the following few lines of code.
                var symSettings = settings.getSymbologySettings(Scandit.Barcode.Symbology.CODE39);
                symSettings.activeSymbolCounts = [
                    7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20
                ];
                // For details on defaults and how to calculate the symbol counts for each symbology, take
                // a look at http://docs.scandit.com/stable/c_api/symbologies.html.

                var picker = new Scandit.BarcodePicker(settings);
                picker.show(success, null, failure);
                picker.startScanning();
            }
        </script>
        <div align="center" valign="center">
            <input type="button" value="scan" onclick="scan()" style="margin-top: 230px;
                    width: 100px; height: 30px; font-size: 1em"/>
        </div>
    </body>
</html>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

<br/>


## Continuous Scaled/Cropped

For an example of how to display the picker in a scaled/cropped configuration, take a look at the extended sample which is part of the SDK archive.

