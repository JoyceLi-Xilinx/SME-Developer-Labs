.PHONY: help

help::
	@echo  " Makefile Usage:"
	@echo  ""
	@echo  "  make build TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to generate the design for specified target"
	@echo  ""
	@echo  "  make run TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to generate and run the design for specified target"
	@echo  ""
	@echo  "  make clean TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to remove the generated non-hardware files for specified target"
	@echo  ""


## platform selection
## Several supported platforms are liste here.
## Or you can add other platfom and set PLATFORM variable to it

PLATFORM := $(AWS_PLATFORM)


## TARGET can be set as:
## sw_emu: software emulation
## hw_emu: hardware Emulation
## hw: hardware run
TARGET := sw_emu

## Below are the names for host executable and xclbin.
## Please keep it unchanged.
HOST_EXE := Filter2D.exe
XO_NAME := Filter2DKernel
XCLBIN := Filter2DKernel.xclbin

SRC_REPO := ./src
BUILD_DIR := ./build


## Host Application files repository

HOST_SRC_CPP := $(SRC_REPO)/host/*.cpp
HOST_HEADER_FILE := $(SRC_REPO)/host/*.h

## Kernel Source Files repository

KERNEL_SRC_CPP := $(SRC_REPO)/kernel/*.cpp
KERNEL_HEADER_FILE := $(SRC_REPO)/kernel/*.h

# Host Compiler Global Settings and Include Libraries

CXXFLAGS += -D__USE_XOPEN2K8 -DC_KERNEL
CXXFLAGS += -I$(XILINX_XRT)/include/
CXXFLAGS += -I$(XILINX_VIVADO)/include/
CXXFLAGS += -I$(SRC_REPO)/kernel
CXXFLAGS += -I$(SRC_REPO)/host
CXXFLAGS += -O0 -g -Wall -fmessage-length=0 -std=c++14 

## CXXLDFLAGS := -fopenmp -L$(XILINX_XRT)/lib/ -Wl,--as-needed -Wl,-rpath,${XILINX_SDX}/lnx64/tools/opencv -L${XILINX_SDX}/lnx64/tools/opencv -L${XILINX_SDX}/lib/lnx64.o
CXXLDFLAGS := -lxilinxopencl -lpthread -lrt -lstdc++ -lmpfr -lgmp -lhlsmc++-GCC46 -lIp_floating_point_v7_0_bitacc_cmodel -lIp_xfft_v9_1_bitacc_cmodel -lIp_fir_compiler_v7_2_bitacc_cmodel -lIp_dds_compiler_v6_0_bitacc_cmodel -L/proj/xbuilds/released/2018.3/GA/2018.3_1207_2324/xbb/xrt/packages/xrt-2.1.0-centos/opt/xilinx/xrt/lib/ -L$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -L$(XILINX_VIVADO)/lnx64/lib/csim -L$(XILINX_VIVADO)/lnx64/tools/dds_v6_0 -L$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -L$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/lib/csim -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/dds_v6_0 -fopenmp -Wl,--as-needed -Wl,-rpath,$(XILINX_SDX)/lnx64/tools/opencv -L$(XILINX_SDX)/lnx64/tools/opencv -lopencv_calib3d -lopencv_contrib -lopencv_core -lopencv_features2d -lopencv_flann -lopencv_gpu -lopencv_highgui -lopencv_imgproc -lopencv_legacy -lopencv_ml -lopencv_objdetect -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_ts -lopencv_video -lopencv_videostab


## Kernel Compiler and Linker Flags

XOCCFLAGS := -t $(TARGET)
XOCCFLAGS += --platform $(PLATFORM) -R1 --save-temps
XOCCFLAGS += -I$(SRC_REPO)/kernel
XOCCFLAGS += --temp_dir $(BUILD_DIR)
XOCCFLAGS += --log_dir $(BUILD_DIR)
XOCCFLAGS += --profile_kernel data:all:all:all:all
#XOCCFLAGS += --nk Filter2DKernel:3:Filter2DKernel_1.Filter2DKernel_2.Filter2DKernel_3

## Host Executable File Generation

$(BUILD_DIR)/$(HOST_EXE): $(HOST_SRC_CPP) $(HOST_HEADER_FILE)
	mkdir -p $(BUILD_DIR)
	xcpp $(CXXFLAGS) $(CXXLDFLAGS) -o $@ $(HOST_SRC_CPP) -lxilinxopencl -lpthread -lrt -lstdc++ -lmpfr -lgmp -lhlsmc++-GCC46 -lIp_floating_point_v7_0_bitacc_cmodel -lIp_xfft_v9_1_bitacc_cmodel -lIp_fir_compiler_v7_2_bitacc_cmodel -lIp_dds_compiler_v6_0_bitacc_cmodel -L/proj/xbuilds/released/2018.3/GA/2018.3_1207_2324/xbb/xrt/packages/xrt-2.1.0-centos/opt/xilinx/xrt/lib/ -L$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -L$(XILINX_VIVADO)/lnx64/lib/csim -L$(XILINX_VIVADO)/lnx64/tools/dds_v6_0 -L$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -L$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/lib/csim -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/dds_v6_0 -fopenmp -Wl,--as-needed -Wl,-rpath,$(XILINX_SDX)/lnx64/tools/opencv -L$(XILINX_SDX)/lnx64/tools/opencv -lopencv_calib3d -lopencv_contrib -lopencv_core -lopencv_features2d -lopencv_flann -lopencv_gpu -lopencv_highgui -lopencv_imgproc -lopencv_legacy -lopencv_ml -lopencv_objdetect -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_ts -lopencv_video -lopencv_videostab

## Kernel XO and Xclbin File Generation

$(BUILD_DIR)/$(XO_NAME).xo: $(KERNEL_SRC_CPP) $(KERNEL_HEADER_FILE)
	mkdir -p $(BUILD_DIR)
	xocc $(XOCCFLAGS) -c -k Filter2DKernel $(KERNEL_SRC_CPP) -o $@

$(BUILD_DIR)/$(XCLBIN): $(BUILD_DIR)/$(XO_NAME).xo
	mkdir -p $(BUILD_DIR)
	xocc $(XOCCFLAGS) -l -o $@ $(BUILD_DIR)/$(XO_NAME).xo 

## Emulation Files Generation

EMCONFIG_FILE = emconfig.json

$(BUILD_DIR)/$(EMCONFIG_FILE):
	 emconfigutil --nd 1  --platform $(PLATFORM) --od $(BUILD_DIR)

# primary build targets

.PHONY: all clean

## build the design without running host application

build: $(BUILD_DIR)/$(HOST_EXE) $(BUILD_DIR)/$(XCLBIN) $(BUILD_DIR)/$(EMCONFIG_FILE)

## build the design and then run host application

run: build
	cp sdaccel.ini $(BUILD_DIR);
ifeq ($(TARGET), hw)
	cd $(BUILD_DIR) && unset XCL_EMULATION_MODE;    ./Filter2D.exe  -x ./$(XCLBIN) -i ../img/test.bmp -n 1;
else
	cd $(BUILD_DIR) && XCL_EMULATION_MODE=$(TARGET) ./Filter2D.exe  -x ./$(XCLBIN) -i ../img/test.bmp -n 1;
endif

## Clean generated files

clean:
	rm -rf $(BUILD_DIR)/$(XCLBIN) $(BUILD_DIR)/$(HOST_EXE) $(BUILD_DIR)/$(EMCONFIG_FILE) $(BUILD_DIR)/$(XO_NAME).xo $(BUILD_DIR)/*.ltx $(BUILD_DIR)/*_$(TARGET).log $(BUILD_DIR)/xocc_*_$(TARGET)_* $(BUILD_DIR)/_x* $(BUILD_DIR)/*.info $(BUILD_DIR)/convolve_fpga_$(TARGET)* $(BUILD_DIR)/link $(BUILD_DIR)/reports/convolve_fpga_$(TARGET)
