# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)

.PHONY: all

all: clean init 
	
	mkdir --parents $(PWD)/build/Boilerplate.AppDir/thunderbird
	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
		libgdk-pixbuf2.0-0 libatk1.0-0 libatk-bridge2.0-0 libgtk2.0-0

	wget --output-document="$(PWD)/build/build.tar.bz2" "https://download.mozilla.org/?product=thunderbird-latest&os=linux64"
	tar -jxvf $(PWD)/build/build.tar.bz2 -C $(PWD)/build

	echo ''                                                                >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                                >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}:\$${APPDIR}/thunderbird"    >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "export LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}"                     >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                                >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                                >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "exec \$${APPDIR}/thunderbird/thunderbird -p default \"\$${@}\""  >> $(PWD)/build/Boilerplate.AppDir/AppRun

	cp --force --recursive $(PWD)/build/thunderbird/* $(PWD)/build/Boilerplate.AppDir/thunderbird

	rm --force $(PWD)/build/Boilerplate.AppDir/*.desktop

	cp --force $(PWD)/AppDir/*.desktop $(PWD)/build/Boilerplate.AppDir/
	cp --force $(PWD)/AppDir/*.png $(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.svg $(PWD)/build/Boilerplate.AppDir/ || true

	export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/Thunderbird.AppImage
	chmod +x $(PWD)/Thunderbird.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://github.com/area-of-dev/apprepo.AppImage/releases/download/latest/apprepo.AppImage \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/13/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
