XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=SDWebImage
PROJECT=$(PROJECT_ROOT)/SDWebImage.xcodeproj
TARGET=SDWebImageFramework
BTOUCH=/Developer/MonoTouch/usr/bin/btouch
SMCS=/Developer/MonoTouch/usr/bin/smcs
MONOXBUILD=/Library/Frameworks/Mono.framework/Commands/xbuild

# 	If you updated the binding to a newer SDWebImage version don't forget to change
#	the Git TAG of newer the version you are binding against
SDVERSIONTAG=3.5

all: SDWebImage.dll

SDWebImage: clean
	git clone --recursive git@github.com:rs/SDWebImage.git && cd SDWebImage && git checkout tags/$(SDVERSIONTAG)
	curl -L https://github.com/dalexsoto/MonoTouch-PSPDFKit-bindings/raw/master/AlexTouch.PSPDFKit/libarclite.a > libarclite.a
	curl -L https://github.com/dalexsoto/MonoTouch-PSPDFKit-bindings/raw/master/AlexTouch.PSPDFKit/libarclite.linkwith.cs > libarclite.linkwith.cs

libSDWebImage.a: SDWebImage
	$(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -configuration Release clean build
	-mv $(PROJECT_ROOT)/build/Release-iphoneos/SDWebImage.framework/Versions/A/SDWebImage $@

SDWebImage.dll: ApiDefinition.cs StructsAndEnums.cs Makefile libSDWebImage.linkwith.cs libSDWebImage.a libarclite.a libarclite.linkwith.cs
	$(MONOXBUILD) /p:Configuration=Release SDWebImage.csproj
	cp bin/Release/SDWebImage.dll SDWebImage.dll

clean:
	-rm -rf list ios SDWebImage/ *.a *.dll *.mdb *.sln libarclite.linkwith.cs bin/ obj/
