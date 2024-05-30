# Steganography Image Embedder

## Introduction

The Steganography Image Embedder script embeds covert messages into an image using steganography. It hides a selected message in the least significant bit (LSB) of the blue color component of the image's pixels. The script uses the `PIL` library for image processing and allows users to choose a message from a predefined code book.

## Features

- **Steganography**: Embeds covert messages in images by modifying the LSB of the blue color component.
- **Predefined Code Book**: Provides a set of predefined messages for embedding.
- **Interactive Selection**: Allows users to select a message to embed into the image.
- **Image Processing**: Uses the `PIL` library to handle image loading and saving.

## Prerequisites

- Python 3.6 or higher.
- Required Python libraries: `PIL` (Pillow).

## Modules

- **PIL**: Provides functions for image processing.

```python
from PIL import Image
```

## Code Book

A dictionary mapping binary codes to covert messages.

```python
code_book = {
    "000": "Dead Drop",
    "001": "Corner of Lexington Ave and E 48th Street",
    "010": "Corner of Madison Ave and E 34th Street",
    "011": "Drop Package in Potted Plant outside Wells Fargo",
    "100": "Drop Package in Gold Garbage Can",
    "101": "12 PM Sharp",
    "110": "7 AM Sharp",
    "111": "Abort if you see a Red Rose"
}
```

## Functions

### `embed_message(img_path, message_code)`

Embeds a message into an image by modifying the LSB of the blue color component of the image's pixels.

- **Parameters:**
  - `img_path` (str): The path to the image file.
  - `message_code` (str): The binary code of the message to embed.
- **Returns:**
  - `None`

#### Example

```python
def embed_message(img_path, message_code):
    try:
        img = Image.open(img_path)
        pix = img.load()
        binary_message = format(int(message_code, 2), '08b')
        idx = 0

        for y in range(img.size[1]):
            for x in range(img.size[0]):
                if idx < len(binary_message):
                    pixel = pix[x, y]
                    new_pixel = []

                    for i in range(3):
                        if i == 2:
                            if binary_message[idx] == '0':
                                new_pixel.append(pixel[i] & 0b11111110)
                            else:
                                new_pixel.append(pixel[i] | 0b00000001)
                            idx += 1
                        else:
                            new_pixel.append(pixel[i])
                    pix[x, y] = tuple(new_pixel)
                else:
                    break

        img.save('hidden_message_image.bmp')
        print("Message embedded successfully. Image saved as 'hidden_message_image.bmp'.")
    except Exception as e:
        print(f"Failed to embed message: {str(e)}")
```

### `main()`

The main function where execution begins. It displays the code book, prompts the user to select a message, and calls the `embed_message` function.

#### Example

```python
def main():
    img_path = 'monalisa.bmp'
    print("Using image file:", img_path)
    print("Please select a message code from the following code book to embed:")
    for code, message in code_book.items():
        print(f"{code}: {message}")
    message_code = input("Enter the message code to embed: ").strip()
    embed_message(img_path, message_code)

if __name__ == "__main__":
    main()
```

## Usage

1. **Install Pillow**: Ensure the `PIL` library is installed.

    ```bash
    pip install pillow
    ```

2. **Prepare the Image**: Place the image file (e.g., `monalisa.bmp`) in the same directory as the script.

3. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

4. **Select a Message**: Follow the on-screen instructions to select a message code from the code book.

5. **Check the Output**: The modified image with the embedded message will be saved as `hidden_message_image.bmp`.

## Example Output

```plaintext
Using image file: monalisa.bmp
Please select a message code from the following code book to embed:
000: Dead Drop
001: Corner of Lexington Ave and E 48th Street
010: Corner of Madison Ave and E 34th Street
011: Drop Package in Potted Plant outside Wells Fargo
100: Drop Package in Gold Garbage Can
101: 12 PM Sharp
110: 7 AM Sharp
111: Abort if you see a Red Rose
Enter the message code to embed: 010
Message embedded successfully. Image saved as 'hidden_message_image.bmp'.
```

## Error Handling

- **File Errors**: If the image file cannot be opened, an error message will be displayed.
- **Embedding Errors**: If there is an issue during the embedding process, an error message will be displayed.

## Security Considerations

- **Confidentiality**: Ensure the confidentiality of the embedded messages.
- **Image Integrity**: Use a copy of the original image to avoid altering the original file.

## FAQs

**Q: What image formats are supported?**
A: The script supports image formats compatible with the `PIL` library.

**Q: Can I use my own code book?**
A: Yes, you can modify the `code_book` dictionary to include your own messages.

**Q: How can I retrieve the embedded message?**
A: This script only embeds messages. You will need a separate script to extract the message.

## Troubleshooting

- **Module Not Found Errors**: Ensure Python is installed correctly and all necessary modules are available. Install any missing modules using `pip`.
- **Image File Not Found**: Ensure the image file is in the same directory as the script.

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
