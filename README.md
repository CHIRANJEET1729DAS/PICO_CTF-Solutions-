# PICO_CTF-Solutions- Chiranjeet Das

# Techniques for Binary Exploitation in PicoCTF

Binary exploitation challenges in PicoCTF are an excellent way to enhance your understanding of low-level programming concepts, memory manipulation, and security vulnerabilities. Below are detailed techniques and approaches to solve several common problem types in this domain.

---

## 1. Format String Vulnerabilities

### **Format\_String\_1 Problem**

This problem focuses on extracting stored values from the stack using format string vulnerabilities.

#### Steps to Solve:

1. Input multiple `%p` format specifiers to flush out the stack values.
2. Convert the output hexadecimal values into ASCII.
3. Reverse the converted ASCII characters.
4. Extract some visible characters of the flag.
5. Rearrange the selected characters and corresponding hexadecimal values to construct the complete flag in the correct format.

---

### **Format\_String\_2 Problem**

This challenge involves changing the value of a variable using buffer overflow techniques.

#### Steps to Solve:

1. Use `objdump` to analyze the binary and find the address of the target variable (e.g., `sus`).
2. Construct a payload:
   - Overflow the buffer with 64 bits of garbage.
   - Navigate to the `sus` address on the stack.
   - Push the desired value (e.g., an address from the vulnerable code).

---

### **Format\_String\_3 Problem**

Here, you exploit the ability to pass a different system function to a value defined for another function in the code.

#### Steps to Solve:

1. Calculate the offsets:
   - Offset for `EBP` inside `setvbuf`: 38.
   - Offset of `setvbuf` from `libc`: `0x7a3f0`.
   - Offset of `system` from `libc`: `0x4f760`.
2. Determine addresses:
   - Address of `libc`: `address_of_setvbuf - offset_of_setvbuf_from_libc`.
   - Address of `system`: `address_of_libc + offset_of_system_from_libc`.
3. Construct the payload using tools like `pwn`:

```python
# Overwrite GOT entry for puts with the system address
payload = fmtstr_payload(start, {elf.sym.got.puts: system_address})
```

---

## 2. Heap Exploitation

### **Heap\_1 Problem**

This challenge involves manually overflowing a variable to overwrite another variable’s value.

#### Steps to Solve:

1. Identify two key variables:
   - `input_data` (e.g., "pico").
   - `safe_var` (e.g., "bico").
2. Calculate how many bits of garbage are needed to fill `input_data` and overflow into `safe_var`.
3. Fill the `input_data` buffer such that the overflow ends with "pico," which gets written into `safe_var`.
4. Verify the flag output.

---

### **Heap\_2 Problem**

This problem is similar to "Heap\_1" but with a simpler approach.

#### Steps to Solve:

1. Find the address of the `win` function using `objdump`.
2. Overflow the `input_data` buffer with garbage, ending with the address of the `win` function in little-endian format.
3. Note: Automated payload scripts may result in incorrect overwriting of the `safe_var` variable, so manual calculation is recommended.

---

## 3. Local-Target Exploitation

This challenge involves analyzing the binary output in hexadecimal format.

#### Steps to Solve:

1. Determine the buffer limit.
2. Overflow the buffer with 1 byte:
   - Try numbers 1-9 and observe how the output increases by 1.
3. Input "A":
   - The output should be 65 (ASCII value of "A").
4. Use this to extract the flag.

---

## 4. Function Pointer Exploitation

### **Picker-IV Problem**

This problem revolves around manipulating a target variable (e.g., `fptr`).

#### Steps to Solve:

1. Find the address of the target variable (`fptr`).
2. Send the address as input in the prescribed format.
3. Extract the flag.

---

## Conclusion

The techniques outlined above can help you solve a wide range of binary exploitation challenges in PicoCTF and similar competitions. Whether it’s format string vulnerabilities, heap overflows, or manipulating function pointers, understanding the underlying mechanics is key to success. As you gain more experience, consider leveraging tools like `pwn` and `objdump` to automate parts of the process and focus on crafting precise exploit



