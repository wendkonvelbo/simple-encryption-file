# simple-encryption-file
import os
from cryptography.fernet import Fernet

# Function to generate and save a key
def generate_key(key_file):
    key = Fernet.generate_key()
    with open(key_file, "wb") as key_out:
        key_out.write(key)

# Function to load the key from a file
def load_key(key_file):
    return open(key_file, "rb").read()

# Function to encrypt a file
def encrypt_file(file_path, key_file):
    if not os.path.exists(file_path):
        print(f"File '{file_path}' not found.")
        return
    
    if not os.path.exists(key_file):
        print(f"Key file '{key_file}' not found. Please generate the key first.")
        return
    
    key = load_key(key_file)
    fernet = Fernet(key)

    with open(file_path, "rb") as file:
        original_data = file.read()

    encrypted_data = fernet.encrypt(original_data)

    with open(file_path, "wb") as file:
        file.write(encrypted_data)

    print(f"{file_path} has been encrypted.")

# Main function to specify the file path and key file
if __name__ == "__main__":
    key_file = "secret.key"  # Path to store the encryption key
    file_path = "D:/cv.docx"  # Path to your cv.docx file

    # Uncomment this line to generate a new key
    generate_key(key_file)  # Run this once to generate the key, then comment it out

    encrypt_file(file_path, key_file)
