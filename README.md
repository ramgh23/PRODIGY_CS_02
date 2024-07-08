# PRODIGY_CS_02
 image encryption tool using pixel manipulation by performing pixel  swapping operation by creating python program that can encrypt and decrypt image using  pixel values  swapping operation by allowing  users to input an image  to perform encryption and decryption.

 Pixel Manipulation:  
Pixel manipulation is altering or modifying pixels of an image to achieve a desired result.  The process of 
modifying the RGB values of each pixel in an image. A digital image is nothing more than data—numbers 
indicating variations of red, green, and blue at a particular location on a grid of pixels. Most of the time, we 
view these pixels as miniature rectangles sandwiched together on a computer screen. With a little creative 
thinking and some lower level manipulation of pixels with code, however, we can display that information in a 
myriad of ways. 

Image Encryption : 
Image encryption, fundamentally defined as the process of transforming a plain image into a coded form that 
can only be deciphered by its intended recipient , has gained increasing importance in response to the growing 
prevalence of image applications and the transmission of images over the internet and open networks. The 
critical information embedded within these images necessitates secure protection. 

Operations: 
Swapping Pixel values 
Pixel swapping in the context of pixel manipulation refers to the process of exchanging the values of pixels in 
an image. This can be done in various ways, such as swapping the color channels of each pixel (e.g., swapping 
the red and blue values) or swapping the positions of two or more pixels in the image.

import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk   

def load_image(path):
    img = Image.open(path)
    return img    #function to load an image

def encrypt_image(img):
    encrypted_img = img.copy()
    pixels = encrypted_img.load()
    
    for i in range(encrypted_img.size[0]):
        for j in range(encrypted_img.size[1]):
            r, g, b = pixels[i, j]
            pixels[i, j] = (g, r, b)  # Swap red and green channels  
            
    return encrypted_img

def decrypt_image(encrypted_img):
    decrypted_img = encrypted_img.copy()
    pixels = decrypted_img.load()
    
    for i in range(decrypted_img.size[0]):
        for j in range(decrypted_img.size[1]):
            g, r, b = pixels[i, j]
            pixels[i, j] = (r, g, b)  # Swap green and red channels back  
             
    return decrypted_img

def save_image(img, path):
    img.save(path)
#function to save and open file
def open_file():
    file_path = filedialog.askopenfilename(filetypes=[("Image files", "*.jpg;*.jpeg;*.png")])
    if file_path:
        img = load_image(file_path)
        img.show()
        return img

def encrypt_and_show():    # function to display encrypted and decrypted image
    global img, encrypted_img
    img = open_file()
    if img:
        encrypted_img = encrypt_image(img)
        encrypted_img.show()

def decrypt_and_show():
    if encrypted_img:
        decrypted_img = decrypt_image(encrypted_img)
        decrypted_img.show()
        return decrypted_img
    else:
        messagebox.showerror("Error", "No encrypted image to decrypt")

def save_encrypted_image():
    if encrypted_img:
        file_path = filedialog.asksaveasfilename(defaultextension=".jpg", filetypes=[("JPEG files", "*.jpg"), ("All files", "*.*")])
        if file_path:
            save_image(encrypted_img, file_path)
    else:
        messagebox.showerror("Error", "No encrypted image to save")

def save_decrypted_image():
    decrypted_img = decrypt_and_show()
    if decrypted_img:
        file_path = filedialog.asksaveasfilename(defaultextension=".jpg", filetypes=[("JPEG files", "*.jpg"), ("All files", "*.*")])
        if file_path:
            save_image(decrypted_img, file_path)
    else:
        messagebox.showerror("Error", "No decrypted image to save")  
# GUI Setup
root = tk.Tk()
root.title("Image Encryption Tool")

frame = tk.Frame(root)
frame.pack(padx=10, pady=10)

open_button = tk.Button(frame, text="Image to Encrypt ", command=encrypt_and_show)
open_button.pack(pady=5)

save_encrypted_button = tk.Button(frame, text="Save Encrypted Image", command=save_encrypted_image)
save_encrypted_button.pack(pady=5)

decrypt_button = tk.Button(frame, text="Decrypt Image", command=decrypt_and_show)
decrypt_button.pack(pady=5)

save_decrypted_button = tk.Button(frame, text="Save Decrypted Image", command=save_decrypted_image)
save_decrypted_button.pack(pady=5)

root.mainloop()
