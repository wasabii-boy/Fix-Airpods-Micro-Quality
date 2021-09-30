<div align="center">
    <h1>
        AirPods Pro fix pour macOS (Big Sur et antérieur)
    </h1>
</div>
C'est un Fix avec Hammerspoon pour corriger le bug de qualité micro sur les Airpods Pro ! Un fix automatique.

## Comment faire ?
1. Il faut télécharger [Hammerspoon](https://www.hammerspoon.org/)
2. Une fois installé et configuré, il suffit d'aller dans la barrre de menu, cliquez sur Hammerspoon et "Open config"
3. Coller ce code

``` 
-- Avoid automatically setting a bluetooth audio input device

lastSetDeviceTime = os.time()
lastInputDevice = nil

function audioDeviceChanged(arg)
    if arg == 'dev#' then
        lastSetDeviceTime = os.time()
    elseif arg == 'dIn ' and os.time() - lastSetDeviceTime < 2 then
        inputDevice = hs.audiodevice.defaultInputDevice()
        if inputDevice:transportType() == 'Bluetooth' then
            internalMic = lastInputDevice or hs.audiodevice.findInputByName('Built-in Microphone')
            internalMic:setDefaultInputDevice()
        end
    end
    if hs.audiodevice.defaultInputDevice():transportType() ~= 'Bluetooth' then
        lastInputDevice = hs.audiodevice.defaultInputDevice()
    end
end

hs.audiodevice.watcher.setCallback(audioDeviceChanged)
hs.audiodevice.watcher.start()
``` 
4. Save
5. "Reload Config"
6. Et voilà



Solution et source (http://ssrubin.com/posts/fixing-macos-bluetooth-headphone-audio-quality-issues-with-hammerspoon.html)
