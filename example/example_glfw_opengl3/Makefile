#
# Cross Platform Makefile
# Compatible with MSYS2/MINGW, Ubuntu 14.04.1 and Mac OS X

#CXX = g++
#CXX = clang++

EXE := example
IMGUI_DIR ?= ../../../imgui
IMPLOT_DIR := ../..

IMGUI_SOURCES := \
	$(IMGUI_DIR)/imgui.cpp \
	$(IMGUI_DIR)/imgui_demo.cpp \
	$(IMGUI_DIR)/imgui_draw.cpp \
	$(IMGUI_DIR)/imgui_tables.cpp \
	$(IMGUI_DIR)/imgui_widgets.cpp \
	$(IMGUI_DIR)/backends/imgui_impl_glfw.cpp \
	$(IMGUI_DIR)/backends/imgui_impl_opengl3.cpp

IMPLOT_SOURCES := \
	$(IMPLOT_DIR)/implot.cpp \
	$(IMPLOT_DIR)/implot_demo.cpp \
	$(IMPLOT_DIR)/implot_items.cpp

SOURCES := main.cpp $(IMGUI_SOURCES) $(IMPLOT_SOURCES)

OBJS := $(addsuffix .o, $(basename $(notdir $(SOURCES))))
UNAME_S := $(shell uname -s)

GLFW_CFLAGS := $(shell pkg-config --cflags glfw3)
GLFW_LIBS := $(shell pkg-config --libs glfw3)

CPPFLAGS := \
	$(GLFW_CFLAGS) \
	-I$(IMGUI_DIR) \
	-I$(IMGUI_DIR)/backends \
	-I$(IMPLOT_DIR)
CXX += -std=c++11
CXXFLAGS := -g -Wall -Wextra
LIBS := $(GLFW_LIBS)

##---------------------------------------------------------------------
## BUILD FLAGS PER PLATFORM
##---------------------------------------------------------------------

ifeq ($(UNAME_S), Linux) #LINUX
	ECHO_MESSAGE := "Linux"
	LIBS += -lGL -ldl
	CPPFLAGS += $(SDL_CFLAGS)
endif

ifeq ($(UNAME_S), Darwin) #APPLE
	ECHO_MESSAGE := "Mac OS X"
	LIBS += -framework OpenGL -framework Cocoa -framework IOKit -framework CoreVideo
	LIBS += -L/usr/local/lib -L/opt/local/lib
	CPPFLAGS += -I/usr/local/include -I/opt/local/include
endif

ifeq ($(OS), Windows_NT)
	ECHO_MESSAGE := "MinGW"
	EXE := $(EXE).exe
	LIBS += -lgdi32 -lopengl32 -limm32
endif

##---------------------------------------------------------------------
## BUILD RULES
##---------------------------------------------------------------------

.PHONY: all clean

all: $(EXE)
	$(info Build complete for $(ECHO_MESSAGE))

clean:
	$(RM) $(EXE) $(OBJS)

%.o:$(IMGUI_DIR)/%.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c -o $@ $<

%.o:$(IMGUI_DIR)/backends/%.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c -o $@ $<

%.o:$(IMPLOT_DIR)/%.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c -o $@ $<

$(EXE): $(OBJS)
	$(CXX) -o $@ $^ $(CXXFLAGS) $(LIBS)

