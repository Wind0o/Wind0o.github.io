# NasV 1.0 tvOS: FFmpeg source and relinking materials

NasV for Apple TV uses FFmpeg 8.0 under LGPL 2.1 or later. FFmpeg copyright remains with its contributors. The included FFmpeg libraries were built with CONFIG_GPL=0, CONFIG_NONFREE=0 and CONFIG_VERSION3=0. Full license text is in licenses/COPYING.LGPLv2.1.

This kit contains the NasV tvOS application object files, package object files and static library inputs needed to relink a modified FFmpeg library with NasV. It corresponds to source revision 5ae141d0262a19c71c9f48296c2939e4d7287f1d and version 1.0. These application objects were compiled with Xcode 27 beta (27A5218g); the App Store archive was separately compiled with Xcode Cloud Xcode 27 (27A266a). The kit does not claim to reproduce Apple's encrypted or signed distribution binary byte-for-byte.

## Build FFmpeg

Download ffmpeg-8.0.tar.xz from the same download page. Its SHA-256 is b2751fccb6cc4c77708113cd78b561059b6fa904b24162fa0be2d60273d27b8e. All 9,888 upstream source files were compared against the source used for the vendored libraries: no modified or missing upstream files were found. Generated configuration files are in build-record. There are no patches to the upstream FFmpeg source.

On macOS with Xcode and the tvOS SDK installed:

    export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
    bash build-record/build-ffmpeg-tvos.sh /absolute/path/to/ffmpeg-8.0.tar.xz

The script creates Vendor/ffmpeg/src from the source archive. To modify FFmpeg, edit that extracted source, then run `make` and `make install` inside it. Do not rerun the extraction script after editing: that script deliberately reconstructs the extracted source directory. It never operates on your app data or media.

The dav1d headers and library needed by the original FFmpeg build are included under Vendor/dav1d. Their BSD license is included. Build paths recorded in config files reflect the original compilation; they are not credentials or required installation paths.

## Relink NasV

For an unchanged-library smoke check:

    bash relink.sh

For a modified FFmpeg build, replace lib/libavformat.a, lib/libavcodec.a, lib/libavutil.a, lib/libswscale.a and lib/libswresample.a with your compatible rebuilt libraries, then run the same command. The result is out/NasV, an arm64 tvOS executable. The kit was tested by relinking all 59 object inputs successfully on macOS with Xcode 27 beta. Apple SDK and system libraries are required and are not redistributed here.

Place the resulting executable in a lawfully obtained NasV 1.0 app bundle in place of its NasV executable, preserving its resources. Installation of a modified app on your own device requires your own applicable Apple signing/provisioning setup. No developer private keys, signing profiles, user NAS configuration, account credentials, or personal media are included in this kit.

The application object files are provided for relinking NasV with modifications to the LGPL-covered library. Modification for that purpose and reverse engineering for debugging those modifications are permitted; applicable open-source license rights take precedence over any conflicting application restriction. This does not grant unrelated redistribution rights in NasV's proprietary code or artwork. Existing third-party rights remain under their respective licenses.

SHA256.json lists the delivered inputs. Support and requests for the corresponding materials: 01057@163.com.
