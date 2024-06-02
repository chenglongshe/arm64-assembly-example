ARMGNU ?= aarch64-linux-gnu
test.elf: test.o
	$(ARMGNU)-ld -Ttext 0x0  -o test.elf   $^
%.o : %.S
	$(ARMGNU)-gcc  -o $@ $<  -g  -c
clean:
	rm *.o *.elf    -f

