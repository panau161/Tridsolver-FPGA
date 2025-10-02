CXX := icpx
CXXFLAGS = -O2 -std=c++17 -Wno-switch-bool
CXXFLAGS_DEBUG = -O0 -g -std=c++17 -Wno-switch-bool # -fsanitize=address -fno-omit-frame-pointer

BOARD = ofs_d5005_usm

SRC := src/ADI2D_FP32.cpp
USM_SRC := src/ADI2D_FP32_usm.cpp

.PHONY: fpga_emu run_emu fpga_emu_usm run_emu_usm clean

fpga_emu: ADI2D_FP32.fpga_emu
fpga_emu_usm: ADI2D_FP32_usm.fpga_emu_usm

hw: ADI2D_FP32.fpga
hw_usm: ADI2D_FP32_usm.fpga

report: ADI2D_FP32_report.a
report_usm: ADI2D_FP32_usm_report.a_usm

run_emu: ADI2D_FP32.fpga_emu
	./ADI2D_FP32.fpga_emu 1 32 32 32
run_emu_usm: ADI2D_FP32_usm.fpga_emu_usm
	./ADI2D_FP32_usm.fpga_emu_usm 1 32 32 4 1

run_hw: ADI2D_FP32.fpga
	./ADI2D_FP32.fpga
run_hw_usm: ADI2D_FP32_usm.fpga
	./ADI2D_FP32_usm.fpga


ADI2D_FP32.fpga_emu: $(SRC)
	$(CXX) $(CXXFLAGS_DEBUG) -fintelfpga $^ -o $@ -DFPGA_EMULATOR=1
ADI2D_FP32_usm.fpga_emu_usm: $(USM_SRC)
	$(CXX) $(CXXFLAGS) -fintelfpga $^ -o $@ -DFPGA_EMULATOR=1


a.o: $(SRC)
	$(CXX) $(CXXFLAGS) -fintelfpga -c $^ -o $@ -DFPGA=1 -Xstarget=$(BOARD)
a_usm.o: $(USM_SRC)
	$(CXX) $(CXXFLAGS) -fintelfpga -c $^ -o $@ -DFPGA=1 -Xstarget=$(BOARD)

ADI2D_FP32.fpga: a.o
	$(CXX) $(CXXFLAGS) -fintelfpga $^ -o $@ -Xshardware -Xstarget=$(BOARD)
ADI2D_FP32_usm.fpga: a_usm.o
	$(CXX) $(CXXFLAGS) -fintelfpga $^ -o $@ -Xshardware -Xstarget=$(BOARD)


dev.o: $(SRC)
	$(CXX) $(CXXFLAGS) -fintelfpga -c $^ -o $@ -DFPGA=1 -Xstarget=$(BOARD)
dev_usm.o: $(USM_SRC)
	$(CXX) $(CXXFLAGS) -fintelfpga -c $^ -o $@ -DFPGA=1 -Xstarget=$(BOARD)

ADI2D_FP32_report.a: dev.o
	$(CXX) $(CXXFLAGS) -fintelfpga -fsycl-link $^ -o $@ -Xshardware -Xstarget=$(BOARD)
ADI2D_FP32_usm_report.a_usm: dev_usm.o
	$(CXX) $(CXXFLAGS) -fintelfpga -fsycl-link $^ -o $@ -Xshardware -Xstarget=$(BOARD)


clean:
	rm -rf *.o *.d *.out *.mon *.emu *.aocr *.aoco *.prj *.fpga_emu *.fpga_emu_buffers ADI2D_FP32.fpga ADI2D_FP32_usm.fpga *.a
