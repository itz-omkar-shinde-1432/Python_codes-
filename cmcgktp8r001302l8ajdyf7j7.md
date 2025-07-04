---
title: ""How I Built a Tor-Based IP Rotator in Python (Change Your IP Every Minute — or Every Second!)""
seoTitle: "Tor IP Rotator in Python: Change Your IP Every Minute Automatically"
seoDescription: "Learn how to build a Python script that automatically changes your public IP address using the Tor network every minute. Perfect for web scraping, privacy,"
datePublished: Sat Jun 28 2025 18:30:35 GMT+0000 (Coordinated Universal Time)
cuid: cmcgktp8r001302l8ajdyf7j7
slug: tor-ip-rotator
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/4PFySnPqAWo/upload/23c49f0fc3cec52db53bb3e01e266147.jpeg
tags: linux, python, privacy, tor, networking, web-scraping, cybersecurity-1

---

# What This Does:

This Python script automatically changes your **public IP address using the Tor network** — every **1 minute** by default (or even faster if you tweak it). It sends a signal to the Tor network to create a new circuit (new IP), then:

* **Fetches the new IP**
    
* **Gets the country** the IP belongs to
    
* **Prints the result to your terminal**
    
* **Logs it into a file called** `ip_log.txt`
    

# Step 1: Install the Required Tools on Linux

```bash
sudo apt update
sudo apt install tor python3-pip -y
```

```bash
pip install stem requests
```

# Step 2: Configure Tor for IP Changing

## Edit Tor's configuration file:

```bash
sudo nano /etc/tor/torrc
```

## Scroll to the bottom and **add this**:

```bash
ControlPort 9051
HashedControlPassword 16: (<paste_your_hash_here>)
CookieAuthentication 0
```

**📝 NOTE:**  
Always create your **own hashed password** for better security.  
Replace the hash inside the brackets (`<paste_your_hash_here>`) with the one you generate.

## How to Create Your Own Tor Password Hash

### 🔹 Step 1: Choose Your Password

Pick any password you want. Example: `mynewpassword123`

### 🔹 Step 2: Generate the Hash in Terminal

Run this command in your Linux terminal:

```bash
tor --hash-password mynewpassword123
```

Replace `mynewpassword123` with your actual password.

### 🔹 Step 3: Copy the Output

You'll get output like this:

```bash
16:FA064B79853907A960053863B6A0AC60A7FEE4F2F701BA00DC4FA43963 (this is for example only)
```

This is your **HashedControlPassword**.

### 🔹 Step 4: Edit Tor Config File

Open Tor’s configuration file:

```bash
sudo nano /etc/tor/torrc
```

Scroll to the bottom and add (replace the hash with yours):

```bash
ControlPort 9051
HashedControlPassword 16:FA064B79853907A960053863B6A0AC60A7FEE4F2F701BA00DC4FA43963
CookieAuthentication 0
```

# Step 3: Restart the Tor Service

After saving changes to the config file, apply them by restarting the Tor service:

```bash
sudo systemctl restart tor
```

✅ Now Tor is ready to accept IP change commands via Python.

## Full Python Script on GitHub

If you don’t want to copy-paste the entire code, no worries!  
You can view and download the complete working script directly from my GitHub:

👉 [**Tor IP Rotator Script on GitHub**](https://github.com/itz-omkar-shinde-1432/Python_codes-/blob/main/tor-ip-rotator)

> The repo includes the full Python script along with comments, logging, and setup instructions.

# **Step 4: Run the Python Script**

Now that everything is set up, it's time to run your Tor IP Rotator script!

In your terminal, run:

```bash
python3 tor_ip_rotator.py
```

You should see output like:

```yaml
🕒 Rotating IP...
🔁 Tor circuit changed!
🌐 IP: 185.xx.xx.xx
📍 Country: Germany
🏢 ISP: N/A
```

🔁 Customize the Rotation Speed (Optional)

Want to rotate IPs faster than every 60 seconds?

Locate these lines at the bottom of your script:

```python
time.sleep(10)  # Let Tor build the circuit
...
time.sleep(50)  # Total wait time: 60 seconds
```

You can adjust them like this:

```python
time.sleep(2)   # Give Tor some time (at least 2–3 sec)
time.sleep(8)   # Change every 10 seconds
```

⚠️ **Note:** Tor might not always give a *new IP every second*. It depends on available exit nodes and network conditions.

# **Common Issues and Fixes**

| Problem | Cause | Fix |
| --- | --- | --- |
| Error changing IP | Wrong password in script | Use the same one you hashed for `torrc` |
| IP Fetch Error | Tor not running | Run: `sudo systemctl start tor` |
| IP doesn’t change | Tor reused same exit node | Wait longer or try a different exit node |
| Permission denied | Not root | No sudo needed for the script, but torrc editing requires `sudo` |

## 🎯 Final Thoughts

This script gives you control over your IP identity using Tor — useful for:

* Safe web scraping
    
* Anonymous testing
    
* Learning how the Tor network works
    

You can extend it by:

* Rotating through custom proxies
    
* Adding a GUI
    
* Running it as a background service
    

## 🙌 Feedback?

If you try this out, let me know how it worked for you!  
Feel free to fork the GitHub repo or drop a comment with suggestions.

> 🔗 [GitHub Repo: Tor IP Rotator](https://github.com/itz-omkar-shinde-1432/Python_codes-/blob/main/tor-ip-rotator)

## Want to See It in Action?

If you'd like to see how the script actually works — including live IP rotation and logging —  
👉 [**Check out the video demo on my LinkedIn**](https://www.linkedin.com/posts/omkar-shinde-3907a927a_python-tor-iprotation-activity-7344799053584433153-y9Nc?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEQa3NwBLGEN1rD-hDYI3cvAXNOHjh0S3XY)

> 🧠 Bonus: I explain how to run it, common errors, and show the changing IPs in real-time.

# Final Usage Tip (Must-Read)

> **Before you start browsing or running your automation tasks:**

1. ✅ **Start the Tor service** (if it's not already running):
    

```bash
sudo systemctl start tor
```

2\. **Run the Python script** to begin IP rotation:

```bash
python3 tor_ip_rotator.py
```

3. 🌐 **Using a regular browser (like Chrome/Firefox)?**  
    You must **configure it to use the Tor proxy** manually:
    
4. * SOCKS5 Proxy: `127.0.0.1`
        
    * Port: `9050`
        
    * Proxy type: `SOCKS5`
        
    * (You can use browser extensions like *FoxyProxy* for easy setup.)
        

> 💡 If you're using the Tor Browser, it's already configured — but this script won't affect the Tor Browser's internal IP; it's meant for **custom Python-based traffic** or **external browsers/tools** you set up to use the Tor proxy.

✅ And that’s it!  
You're now ready to browse, automate, or experiment safely through the Tor network with dynamic IP rotation.

# 🌍 Note on IP-Check API (optional)

The script currently uses the free API:

```bash
https://api.myip.com
```

This usually returns your IP address and country, but:

> ⚠️ **Sometimes it may fail or rate-limit your requests** — especially with frequent IP changes.

### 🔄 What You Can Do:

Feel free to replace it with any other IP info API in the script. Here are a few alternatives:

| API URL | Returns | Free? |
| --- | --- | --- |
| [https://ipinfo.io/json](https://ipinfo.io/json) | IP, country, city, ISP, etc. | ✅ |
| [https://api.ipify.org?format=json](https://api.ipify.org?format=json) | IP only | ✅ |
| [https://ip-api.com/json/](https://ip-api.com/json/) | IP, location, ISP | ✅ *(with limits)* |

Update this line in your script:

```python
res = requests.get("https://api.myip.com", proxies=proxies, timeout=10)
```

To:

```python
res = requests.get("https://ipinfo.io/json", proxies=proxies, timeout=10)
```

And update how you extract values from the JSON response accordingly.

> 💡 This flexibility lets you tailor the output for your use case — like showing ISP, region, or full geo-info.

## 💬 Need Help?

Encountering an issue while setting things up?  
I'm happy to help!

👉 [**Message me on LinkedIn**](https://www.linkedin.com/in/omkar-shinde-3907a927a/) and I’ll get back to you as soon as I can.

> 📩 Whether it’s a Tor config error, IP not rotating, or proxy setup confusion — feel free to reach out.