# Copyright 1996-2021 Cyberbotics Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Webots Makefile system
#
# You may add some variable definitions hereafter to customize the build process
# See documentation in $(WEBOTS_HOME_PATH)/resources/Makefile.include


CXX_SOURCES = \
  AccelerometerGroupBox.cpp \
  GyroGroupBox.cpp \
  MagnetometerGroupBox.cpp \
  FireBird6Representation.cpp \
  EncoderGroupBox.cpp \
  Gui.cpp \
  MainWidget.cpp \
  RemoteControlWidget.cpp \
  entry_points.cpp

HPP_FILES_TO_MOC = \
  RemoteControlWidget.hpp

USE_C_API = true

CFLAGS = -DQT_NO_OPENGL
QT_UTILS = $(WEBOTS_HOME)/resources/projects/libraries/qt_utils
INCLUDE = -I"$(QT_UTILS)"
LIBRARIES = -L"$(QT_UTILS)" -lqt_utils

null :=
space := $(null) $(null)
WEBOTS_HOME_PATH=$(subst $(space),\ ,$(strip $(subst \,/,$(WEBOTS_HOME))))
include $(WEBOTS_HOME_PATH)/resources/Makefile.include

# Qt linking

CFLAGS += -std=c++11
MOC_SOURCES = $(addprefix $(BUILD_DIR)/,$(notdir $(HPP_FILES_TO_MOC:.hpp=.moc.cpp)))
MOC_DIRECTORIES = $(sort $(dir $(HPP_FILES_TO_MOC)))
OBJECTS += $(addprefix $(BUILD_GOAL_DIR)/,$(notdir $(MOC_SOURCES:.cpp=.o)))

ifeq ($(OSTYPE),windows)
 CFLAGS += -D_USE_MATH_DEFINES
 QT_INCLUDE_DIR = $(WEBOTS_HOME)/include/qt
 MOC = moc
 MOC_PLATFORM_FLAGS = -D_WIN32
 DYNAMIC_LIBRARIES += -L"$(WEBOTS_HOME)/msys64/mingw64/bin" -lQt5Core -lQt5Gui -lQt5Widgets -lQt5Concurrent
 INCLUDE += -isystem "$(QT_INCLUDE_DIR)/QtCore" -isystem "$(QT_INCLUDE_DIR)/QtGui" -isystem "$(QT_INCLUDE_DIR)/QtWidgets" -isystem "$(QT_INCLUDE_DIR)/QtConcurrent"
endif

ifeq ($(OSTYPE),linux)
 QT_INCLUDE_DIR = $(WEBOTS_HOME)/include/qt
 QT_LIB_DIR = $(WEBOTS_HOME)/lib/webots
 MOC = "$(WEBOTS_HOME)/bin/qt/moc"
 MOC_PLATFORM_FLAGS = -D__linux__
 DYNAMIC_LIBRARIES += -L"$(QT_LIB_DIR)" -lQt5Core -lQt5Gui -lQt5Widgets -lQt5Concurrent
 INCLUDE += -isystem "$(QT_INCLUDE_DIR)/QtCore" -isystem "$(QT_INCLUDE_DIR)/QtGui" -isystem "$(QT_INCLUDE_DIR)/QtWidgets" -isystem "$(QT_INCLUDE_DIR)/QtConcurrent"
endif

ifeq ($(OSTYPE),darwin)
 WBCFLAGS += -Wno-unknown-pragmas
 FRAMEWORKS_DIR = $(WEBOTS_HOME)/Contents/Frameworks
 INCLUDE += -F"$(FRAMEWORKS_DIR)"
 FRAMEWORKS += -F"$(FRAMEWORKS_DIR)"
 DYNAMIC_LIBRARIES += -bind_at_load $(FRAMEWORKS)
 MOC = "$(WEBOTS_HOME)/bin/qt/moc"
 MOC_PLATFORM_FLAGS = -D__APPLE__
 FRAMEWORKS += -framework QtCore -framework QtGui -framework QtWidgets -framework QtConcurrent
endif

vpath %.cpp $(BUILD_DIR)
vpath %.hpp $(MOC_DIRECTORIES)

.PRECIOUS: $(MOC_SOURCES)

$(BUILD_DIR)/%.moc.cpp: %.hpp
	@echo "# moc-generating" $(notdir $<)
	$(SILENT)$(MOC) $(MOC_PLATFORM_FLAGS) $(INCLUDE:-isystem:-I) $< -o $@

$(BUILD_GOAL_DIR)/$(MAIN_TARGET): $(OBJECTS)
