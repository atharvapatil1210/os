# OS

# **🛠 Step 1: Set Up Your Development Environment**
We'll need a Linux system for development. If you're on **Windows**, install **WSL** or use a Linux VM.

### **🔹 Install Essential Tools**
Run this command to install the necessary tools:  
```sh
sudo apt update && sudo apt install build-essential nasm qemu gcc-multilib gdb
```
✅ **GCC** → Compiler for C code  
✅ **NASM** → Assembler for low-level Assembly  
✅ **QEMU** → Emulator to run our OS  
✅ **GDB** → Debugger for debugging OS code  

---

# **🛠 Step 2: Write a Basic Bootloader (Assembly)**
The bootloader is the first thing that runs when the computer starts. It sets up the CPU and loads the kernel.

### **🔹 Create a Bootloader File (`boot.asm`)**
Open a new file and write:
```assembly
[BITS 16]        ; Switch to 16-bit real mode
[ORG 0x7C00]     ; Bootloader starts at memory 0x7C00

mov ah, 0x0E    ; BIOS text mode function
mov al, 'H'     ; Character to print
int 0x10        ; Call BIOS interrupt
mov al, 'i'
int 0x10

jmp $           ; Infinite loop (halts execution)

times 510-($-$$) db 0  ; Fill remaining space with zeros
dw 0xAA55       ; Boot sector signature
```

---

# **🛠 Step 3: Compile and Create a Bootable Image**
### **🔹 Assemble the Bootloader**
Run:
```sh
nasm -f bin boot.asm -o boot.bin
```
This creates `boot.bin`, a 512-byte bootloader.

### **🔹 Run it in QEMU**
```sh
qemu-system-x86_64 -drive format=raw,file=boot.bin
```
🔥 If you see "Hi" printed on the screen, your bootloader is working!

---

