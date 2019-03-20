# ITSEasyReader

This project uses Cocoapods as a dependency manager. To properly run the project you need to install Cocoapods:
```
sudo gem install cocoapods
```
Create a Podfile in your project directory:
```
cd your_project_directory
pod init
```
Open the file with any text editor and recplace all current content with the following:
```
use_frameworks!
platform :ios, '11.0'

target 'LoveInASnap' do
  use_frameworks!
  pod 'TesseractOCRiOS'
end

post_install do |installer|
    installer.pods_project.targets.each do |target|
        if target.name == 'TesseractOCRiOS' 
            target.build_configurations.each do |config|
                config.build_settings['ENABLE_BITCODE'] = 'NO'
            end
            header_phase = target.build_phases().select do |phase|
                phase.is_a? Xcodeproj::Project::PBXHeadersBuildPhase
            end.first

            duplicated_header_files = header_phase.files.select do |file|
                file.display_name == 'config_auto.h'
            end

            duplicated_header_files.each do |file|
                header_phase.remove_build_file file
            end
        end
    end
end
```

This tells CocoaPods that you want to include the TesseractOCRiOS framework as a dependency for your project, and resolves a build error where there are multiple header files created (If you do not paste the second paragraph you will see the error).
Finally, in the terminal, within the same directory as earlier, type
```
pod install

```

This project uses an  an Objective-C wrapper for Tesseract OCR written by gali8: ref to https://github.com/gali8/Tesseract-OCR-iOS

A steo to step tutorial can be found here by Lyndsey Scott: https://www.raywenderlich.com/306-tesseract-ocr-tutorial-for-ios

Make sure the build options for both project and pod are ios 11.0, and the simulator used is ios 11.0

