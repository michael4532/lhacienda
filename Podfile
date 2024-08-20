# Uncomment this line to define a global platform for your project
platform :ios, '14.0'


# CocoaPods analytics sends network stats synchronously affecting flutter build latency.
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

project 'Runner', {
  'Debug' => :debug,
  'Profile' => :release,
  'Release' => :release,
}

def flutter_root
  generated_xcode_build_settings_path = File.expand_path(File.join('..', 'Flutter', 'Generated.xcconfig'), __FILE__)
  unless File.exist?(generated_xcode_build_settings_path)
    raise "#{generated_xcode_build_settings_path} must exist. If you're running pod install manually, make sure flutter pub get is executed first"
  end

  File.foreach(generated_xcode_build_settings_path) do |line|
    matches = line.match(/FLUTTER_ROOT\=(.*)/)
    return matches[1].strip if matches
  end
  raise "FLUTTER_ROOT not found in #{generated_xcode_build_settings_path}. Try deleting Generated.xcconfig, then run flutter pub get"
end

require File.expand_path(File.join('packages', 'flutter_tools', 'bin', 'podhelper'), flutter_root)

flutter_ios_podfile_setup



target 'Runner' do
  use_frameworks!
  use_modular_headers!
  pod 'PhoneNumberKit/PhoneNumberKitCore', :git => 'https://github.com/marmelroy/PhoneNumberKit', :tag => '3.5.10'

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
end

post_integrate do |installer|
  compiler_flags_key = 'COMPILER_FLAGS'
  project_path = 'Pods/Pods.xcodeproj'

  project = Xcodeproj::Project.open(project_path)
  project.targets.each do |target|
    target.build_phases.each do |build_phase|
      if build_phase.is_a?(Xcodeproj::Project::Object::PBXSourcesBuildPhase)
        build_phase.files.each do |file|
          if !file.settings.nil? && file.settings.key?(compiler_flags_key)
            compiler_flags = file.settings[compiler_flags_key]
            file.settings[compiler_flags_key] = compiler_flags.gsub(/-DOS_OBJECT_USE_OBJC=0\s*/, '')
          end
        end
      end
    end
  end
  project.save()
end
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)

    # ADD THE NEXT SECTION
    target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',
        'AUDIO_SESSION_MICROPHONE=0'
      ]
    end

  end
end
