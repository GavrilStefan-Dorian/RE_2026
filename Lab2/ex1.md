**Exercise 1**: walk a PE file by hand (~20 minutes)
Open valid/console32.exe in a hex editor: GView from Lab 1, HxD, ImHex, or xxd / Format-Hex. Write down the answers before writing any code.

1. What bytes are at offset 0x3C? What is e_lfanew?
    - at 0x3C -> HEX: 00F0 = e_lfanew
    - value at e_lfanew -> DWORD: 00004550h
2. Go to e_lfanew. Read the 4 signature bytes and the first 8 bytes of the COFF header. What are Machine, NumberOfSections and TimeDateStamp? Convert the timestamp to a date (any online Unix timestamp converter works).
    - 4 signature bytes: PE\0\0
    - Machine: 014C -> x86
    - NumberOfSections: 5
    - TimeDateStamp: I32 -> 1,792,152,873 -> Fri Oct 16 2026 15:14:33 GMT+0300 (Eastern European Summer Time)
3. Where does the optional header start? Is the file PE32 or PE32+? What are AddressOfEntryPoint and ImageBase?
    - Offset/Pos: 108
    - Hex: 10B -> PE32
    - AddressOfEntryPoint: DWORD: 0000125Ah
    - ImageBase: DWORD: 00400000H -> EXE
4. Where does the section table start? What are the name, VirtualAddress and PointerToRawData of the first section?
    - SizeOfOptionalHeader: POS: 104 and WORD: E0 -> PE32
    - Section table starts at e_lfanew + 4 + 20 + SizeOfOptionalHeader = 108 + E0 = 1E8 
    - Name: .text, VirtualAddress: 1000 ( at offset 1F4 ), PointerToRawData: 400 ( at offset 1FC )
5. At what file offset is the first instruction of the program? At what VA will a debugger show it?
    - first instruction's file offset: AddressOfEntryPoint (RVA) - VirtualAddress + PointerToRawData = 125A - 1000 + 400 = 65A
    - first instruction's VA: ImageBase + 125A = 400000 + 125A = 40125A
6. Look at the first bytes at that file offset. The byte E8 is a call and E9 is a jmp: can you guess which compiler built the file? (Course 1, slide "identifying the compiler")
    - E8 A5 02 00 00: call 0x904
    - E9 71 FE FF FF: jmp 0x4d5
    - the only example in the course with both call and jmp instructions is MSVC x64


