--

# File Compression Using Huffman's Algorithm

## About

Huffman’s Algorithm is an efficient technique for file compression and decompression. This program strictly adheres to the Huffman algorithm by reading the most frequent characters from an input file and replacing them with shorter binary codewords. The original file can be reconstructed perfectly, without any data loss.

## Usage

### Compression:
```bash
./encode <file to compress>
```
This will generate an output file named `<inputfile>.spd`.

### Decompression:
```bash
./decode <file to decompress>
```

## File Structure

The compressed file structure is as follows:

| Field                       | Description                                             |
|-----------------------------|---------------------------------------------------------|
| N (1 byte)                  | Total number of unique characters                       |
| Character (1 byte)          | The character                                           |
| Binary Codeword (MAX bytes) | The corresponding binary codeword                       |
| p (1 byte)                  | Padding count (number of bits added to align to bytes)  |
| DATA                        | The compressed data                                     |

*Note*: Padding is added to ensure the file size is a whole number of bytes. For example, if the file size is 4 bytes + 3 bits, it will be padded by 5 bits to make it 5 bytes.

## Example

For the text: `aabcbaab`

| Content                           | Description                              |
|-----------------------------------|------------------------------------------|
| 3                                 | N = 3 (unique characters: a, b, c)       |
| a "1"                             | Character 'a' with codeword "1"          |
| b "01"                            | Character 'b' with codeword "01"         |
| c "00"                            | Character 'c' with codeword "00"         |
| 4                                 | Padding count: 4                         |
| [0000]                            | 4 zeroes added as padding                |
| [1] [1] [01] [00] [01] [1] [1] [01]| Actual data with codewords replacing chars|

## Algorithm

### Step-by-Step Process:

1. **Pass 1:** Read the input file.
2. Create a sorted linked list of characters from the file based on frequency:
   - For each character `ch` in the file:
     - If `ch` is already in the linked list, increase its frequency and re-sort the list.
     - If `ch` is not in the list, add a new node at the beginning with a frequency of 1.
3. Construct the Huffman tree from the linked list:
   - Create a new node `q`, joining the two least frequent nodes as its left and right children.
   - Insert node `q` back into the list in ascending order.
   - Repeat this process until only one node remains, which becomes the root of the Huffman tree.
   - Traverse the tree in pre-order, assigning codewords to each node and recreating the linked list with the leaf nodes.
4. Write the mapping table (character to codeword) to the output file.
5. **Pass 2:** Read the input file again.
6. Replace each character in the input file with its corresponding codeword in the output file by looking it up in the mapping table or linked list.
7. End.

## Development

Sannidhi Hemanth


---
