<div align="center">

# 🕸️ Maltego: Harvesting Email Addresses for a Domain on Ubuntu

**Installing Maltego Graph on Ubuntu and using it to find the email addresses linked to `networkwalks.com`**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Tool-Maltego%20Graph%204.14.0-1679A7?style=flat-square" />
  <img src="https://img.shields.io/badge/Role-Pentester%20(OSINT%20Recon)-C00000?style=flat-square" />
  <img src="https://img.shields.io/badge/Finding-info%40networkwalks.com-C8570A?style=flat-square" />
  <img src="https://img.shields.io/badge/Phase-1%20Footprinting-238F89?style=flat-square" />
  <img src="https://img.shields.io/badge/NetworkWalks-B082-C00000?style=flat-square" />
</p>

---

## 📌 Project Overview

This is the optional Week 2 lab **W2-PM3: Footprinting with Maltego** from the Networkwalks Cybersecurity internship. The lab has two tasks:

1. Download and install Maltego.
2. Find the email addresses related to the target organization's domain, `networkwalks.com`, with due permission.

The lab guide is written for **Windows**. I did the lab on my **Ubuntu** machine, so the install steps are different from the guide: I installed the `.deb` package, added Java myself, and had to fix a few problems before Maltego was usable (see the Challenges section). I ran everything on 13 November 2026. The lab has no automated score or result card, so there is no score here.

The only thing I ran against the target was one transform that looks for email addresses on the domain. No exploitation or access to any data was attempted.

| 🔎 Item | ✅ Result |
| ------- | --------- |
| Tool and version | Maltego Graph (Desktop) 4.14.0 |
| Install method (Ubuntu) | `sudo apt install ./Maltego.v4.14.0.deb`, plus OpenJDK 17 |
| Target | `networkwalks.com` |
| Transform used | `[Utilities] To Emails @domain [Search Engine]` |
| Email found | `info@networkwalks.com` |
| Transform time and cost | About 15 seconds, 10 credits consumed |

---

## 🎯 Objectives

- Install Maltego on a system the lab does not cover (Ubuntu instead of Windows)
- Create a Maltego ID and complete the first-run configuration
- Install the transforms I need from the Maltego Data Hub
- Add a Domain entity to a graph and point it at the target
- Run an email-related transform and read the result on the graph
- Document the setup problems honestly, not just the final result

---

## 🛡️ Why This Matters for Security

Maltego is an OSINT tool: it turns public data about a domain, such as its email addresses, into a graph that shows how the pieces connect. In the early phase of a security test, the email addresses of an organization show which names and contact points are public, and they are the starting point for later phases such as phishing exercises. A defender runs the same transform on their own domain to see what an outsider can see.

The result of this lab is small, but the habit matters: start with one entity, run one transform, and read what came back.

> ⚠️ **Important:** Only run transforms against domains you own or have written permission to test. Here the target was `networkwalks.com`, the domain named in the lab for this purpose. Transforms run through Maltego's data services and use credits, so each run is also a request made to third-party services.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| ------------ | ---------------- |
| 🌐 Platform | My own machine, no online lab |
| 🖥️ Analyst PC | Ubuntu 24.04 |
| 🛠️ Tool | Maltego Graph (Desktop) 4.14.0, installed from the `.deb` package (227 MB) |
| ☕ Java | OpenJDK 17.0.21 (installed by me, see Challenges) |
| 🔑 Account | Maltego ID (created in the browser, online activation) |
| 🧰 Data Hub items | 2 items installed, 672 transforms |
| 🎯 Target | `networkwalks.com` |
| 🔒 Privacy | Browser tabs, bookmarks and address bars are cropped out; my machine's hostname and my Google account picture are blacked out |
| 🏆 Scoring | None |

---

# 🪜 Lab Procedure

## Step 1. Install Maltego and Java (Task 1)
The guide's Windows installer installs Java for you. On Ubuntu I installed the Maltego `.deb` file with `apt`:

```bash
sudo apt install ./Maltego.v4.14.0.deb
```

The package installed. `apt` also printed a notice that the download was done "unsandboxed as root" because the `_apt` user could not read the file in my Downloads folder; it did not stop the install.

![apt install of the Maltego deb](screenshots/pm3-01-task1-apt-install-deb.png)

When I tried to start Maltego, I got a **Java Not Found** error, and running it from the terminal gave the same message ("Cannot find Java"). `dpkg -l maltego` shows the package itself is installed.

```bash
dpkg -l maltego
sudo maltego
```

![Java Not Found](screenshots/pm3-02-task1-java-not-found.png)

I installed OpenJDK 17, the Java version the lab guide says Maltego prefers. The screenshot shows the end of the installation and `java -version` reporting OpenJDK 17.0.21.

```bash
java -version
```

![OpenJDK 17 installed](screenshots/pm3-03-task1-openjdk-17-installed.png)

## Step 2. First run: activation and Maltego ID (Task 1)
After that, Maltego started and showed its activation options. I chose **Maltego ID**, then **Online Activation (Default)**, and accepted the license agreement.

![Activation options](screenshots/pm3-04-task1-maltego-activation-options.png)

![Online activation](screenshots/pm3-05-task1-online-activation.png)

![License agreement](screenshots/pm3-06-task1-license-agreement.png)

The next page asks me to log in through the browser. Maltego shows a login link and a **Browser Login** button, and then waits for the browser to confirm the login.

![Login link options](screenshots/pm3-07-task1-login-link-options.png)

![Waiting for browser login](screenshots/pm3-08-task1-waiting-for-browser-login.png)

I looked up how to open the login page by hand on Linux. The AI answer I got says the automatic browser launch often fails on Linux desktops and that the login URL should be copied from Maltego and pasted into the browser.

![AI answer: open the login page manually](screenshots/pm3-09-task1-ai-open-login-page-manually.png)

In the browser I got the Maltego login page. I did not have an account yet, so I used **Create ID**, filled the form (the screenshot shows the empty form) and logged in. The browser then showed "Authentication complete" and Maltego reported "Browser login was successful".

![Maltego login page](screenshots/pm3-10-task1-maltego-login-page.png)

![Create Maltego ID](screenshots/pm3-11-task1-create-maltego-id.png)

![Authentication complete](screenshots/pm3-12-task1-authentication-complete.png)

![Browser login successful](screenshots/pm3-13-task1-browser-login-successful.png)

The rest of the setup wizard used the lab's default settings: transforms from **Utilities**, accepting the data sources terms, the default system browser and **Normal** privacy mode.

![Select data sources](screenshots/pm3-14-task1-select-data-sources.png)

![Data sources terms](screenshots/pm3-15-task1-data-sources-terms.png)

![Web browser options](screenshots/pm3-16-task1-web-browser-options.png)

![Privacy mode options](screenshots/pm3-17-task1-privacy-mode-options.png)

When the wizard finished, Maltego showed the Data Hub and a Product Tour popup.

![Product tour popup](screenshots/pm3-18-task1-product-tour-popup.png)

## Step 3. Fixing the display and the empty Entity Palette (Task 1)
Maltego did not work properly right away. I searched for help and the AI answer suggested closing all Maltego processes and starting it again with the GPU acceleration turned off. The captures below show the suggested commands. I describe what I could and could not confirm in the Challenges section.

```bash
pkill -9 -f maltego
maltego -J-Dprism.order=sw -J-Dsun.java2d.opengl=false --nosplash
```

![AI answer: GPU fix steps](screenshots/pm3-19-task1-ai-gpu-fix-steps.png)

![AI answer: launch command](screenshots/pm3-20-task1-ai-launch-command.png)

Maltego then showed a blank graph, but the **Entity Palette** was empty, and searching it for "domain" found nothing.

![Blank graph and empty palette](screenshots/pm3-21-task1-blank-graph-empty-palette.png)

![Domain search with no result](screenshots/pm3-22-task1-domain-search-no-result.png)

The AI answer explained that Maltego 4.14.0 does not come with entities and transforms ready to use, and that they have to be installed from the **Data Hub**.

![AI answer: why the palette is empty](screenshots/pm3-23-task1-ai-empty-palette-explanation.png)

I opened the **Maltego Data Hub**. At first it said 0 Hub items installed (0 transforms). After I installed items from the hub list, it says **2 Hub items installed (672 Transforms)**; the highlighted cards are Cyber Threat Intelligence and Utilities.

![Data Hub](screenshots/pm3-24-task1-data-hub.png)

![Data Hub partners, 0 installed](screenshots/pm3-25-task1-data-hub-partners.png)

![Data Hub, 2 items installed](screenshots/pm3-26-task1-data-hub-items-installed.png)

**Answer: Maltego Graph 4.14.0 installed and working on Ubuntu, with the Data Hub transforms installed.**

## Step 4. Add the Domain entity (Task 2)
In the Entity Palette I searched for "domain" and dragged **Domain** onto the graph. A new Domain entity is named `maltego.com` by default.

![Domain entity added](screenshots/pm3-27-task2-domain-entity-added.png)

I double-clicked the entity and changed the **Domain Name** to `networkwalks.com`.

![Rename the entity](screenshots/pm3-28-task2-rename-entity.png)

## Step 5. Run the email transform (Task 2)
I right-clicked the entity, searched the transform list for "email" and ran **`[Utilities] To Emails @domain [Search Engine]`**, the transform the lab uses. The list also offers other email transforms, for example the Cyber Threat Intelligence ones based on Whois and RDAP, which I did not run.

![Email transforms list](screenshots/pm3-29-task2-email-transforms.png)

Maltego asked for the transform's inputs. I left **Extract URLs** unchecked and clicked **Run!**.

![Required inputs](screenshots/pm3-30-task2-transform-required-inputs.png)

The graph now shows `networkwalks.com` linked to **`info@networkwalks.com`**. The output log shows the transform running on 1 entity, consuming 10 credits and finishing in about 15 seconds. The status bar says **3 entities, 2 links**. The graph window in my capture only shows the first email entity; the third entity is below the visible part of the graph, so I cannot name it from this screenshot.

![Email result](screenshots/pm3-31-task2-email-result.png)

**Answer: `info@networkwalks.com` (visible in the capture). The graph reports 3 entities and 2 links in total.**

---

# 📊 Results

| # | 🎯 Task | ✅ Answer | 🔎 How I Found It |
| - | ------- | --------- | ------------------ |
| 1 | Download and install Maltego | Maltego Graph 4.14.0 running on Ubuntu | `.deb` install, OpenJDK 17, Maltego ID login, Data Hub install |
| 2 | Find the email addresses of `networkwalks.com` | `info@networkwalks.com` visible; graph shows 3 entities and 2 links | Domain entity, `[Utilities] To Emails @domain [Search Engine]` |

*Footnote: the lab has no result card, so no score is shown. The answer to Task 2 comes from the graph and the output log in my last screenshot.*

### Timeline (from the timestamps of my screenshots, 13 November 2026)
| ⏱️ Time | 🔄 What happened |
| ------- | ---------------- |
| 12:32 | `.deb` installed with `apt` |
| 12:33 to 12:36 | Java Not Found, then OpenJDK 17 installed, Maltego opens |
| 12:38 to 12:39 | Activation options, license, login link |
| 12:46 to 12:54 | Browser login and Maltego ID, login successful |
| 12:55 to 13:00 | Setup wizard finished, Data Hub and product tour |
| 13:05 to 13:09 | Display fix suggestions, blank graph with an empty palette |
| 13:13 to 13:19 | Data Hub items installed |
| 13:20 to 13:23 | Domain entity renamed, email transform run, result |

---

# 🔭 Observations Beyond the Task Questions

These are things I noticed in my screenshots while documenting. The lab does not ask for them.

1. **Most of my time went on setup, not on the task.** The screenshots run from 12:32 to 13:23, about 51 minutes, while the email transform itself finished in about 15 seconds.
2. **The email Maltego found is also in my PM1 WhatWeb output.** The `whatweb` result from the Week 2 footprinting lab listed `info@networkwalks.com` too, so two different tools show the same public address.
3. **A new Domain entity starts as `maltego.com`.** If I had not renamed it, a transform would have been aimed at the wrong domain. I renamed it before running the email transform.
4. **The transform uses credits.** The log shows 10 credits consumed and a remaining balance starting with 190 (the line is cut off in my capture).
5. **The Data Hub shows an error counter.** After the install, the bottom right of the window shows an "Unexpected Exception" message and a count of 128. I cannot tell from the capture what caused it. Maltego kept working.
6. **Many email transforms exist.** The list for my domain entity includes several Cyber Threat Intelligence transforms (parsed Whois, RDAP, Whois). I only ran the Utilities one the lab uses, so I cannot say if the others return more addresses.

---

# 🐞 Challenges Encountered

* **Issue (Task 1, Java missing):** After installing the Maltego `.deb`, Maltego would not start and showed "Java Not Found"; `sudo maltego` printed "Cannot find Java".
  **Resolution:** I installed OpenJDK 17 (screenshot 3 shows the end of that install and `java -version` working).
  **What I'll do differently:** Check the tool's prerequisites in its documentation before installing. The Windows installer in the guide installs Java automatically; the Linux package does not.

* **Issue (Task 1, browser login):** The login step needed my browser and I was not sure the automatic launch would work on Linux.
  **Research:** I looked it up (screenshot 9). The AI answer says the automatic launch often fails on Linux desktops and to copy the login URL into the browser by hand. My screenshots show the login page, my Maltego ID signup and "Authentication complete", so the login worked, but they do not show whether I needed the manual route.
  **What I'll do differently:** Note what exactly fails when it happens, so the documentation is exact.

* **Issue (Task 1, display problem):** Maltego had a display problem on first use. I looked for help and the AI answer suggested restarting it with the GPU turned off (screenshots 19 and 20). My captures do not show the command I actually used for the session that worked, so I am not claiming it.
  **What I'll do differently:** Keep the terminal in the screenshot when I start a tool, so the launch command is part of the evidence.

* **Issue (Task 1, empty Entity Palette):** The graph opened, but the Entity Palette was empty and "domain" found nothing.
  **Research:** The AI answer (screenshot 23) said Maltego 4.14.0 does not come with transforms preinstalled and they must be installed from the Data Hub.
  **Resolution:** I installed items from the Data Hub. It went from 0 items installed to **2 items and 672 transforms**, and the Domain entity could then be found and used.
  **What I'll do differently:** After installing a tool like this, check the Data Hub first.

* **No problems with Task 2 itself.** Once the setup was working, adding the entity and running the transform worked first time. I used AI answers as help for the problems above, and the screenshots of those answers are included.

---

# 💡 Key Learnings

1. Installing a tool on a different OS from the guide is part of the job. The same lab needed different steps on Ubuntu, and Java was the first gap.
2. Reading the error message first saves time. "Java Not Found" told me exactly what was missing.
3. Maltego does nothing useful until the transforms are installed. The Data Hub is a required step, not an extra.
4. A graph tool is only as good as its first entity. Renaming the default `maltego.com` entity to the real target before any transform is a scope check, not a detail. This connects to my WhatWeb typo in PM1.
5. One small result can come from a lot of setup. The email transform took 15 seconds; the setup took most of the 51 minutes.
6. OSINT tools often use credits and external services, so every run is a request I should be able to justify.
7. Two different tools (WhatWeb and Maltego) showing the same email address increases my confidence that it is the public contact address.
8. Next time I will keep the launch command and the Data Hub state in my screenshots, so I can document exactly what fixed each problem.

---

# 🔐 Security & Ethical Use

This work is for education only. The only target was `networkwalks.com`, the domain named in the lab, and I ran a single transform that looks for email addresses. Do not run Maltego transforms against domains or organizations you do not own or do not have written permission to test. Email addresses are personal data in many cases, so collect only what a test needs and handle it carefully.

---

# 🔗 Tools & Resources

- **Maltego:** https://www.maltego.com
- **NetworkWalks:** https://networkwalks.com

---

# 👤 Author
**Philip Ajik**
Cybersecurity Student (Batch B082) — Networkwalks Academy
LinkedIn: https://www.linkedin.com/in/philip-ajik-a22944384

---

## 📌 Project Information
**Program Name:** Cybersecurity at Networkwalks | **Task:** W2-PM3 (optional) | **Project:** Footprinting with Maltego | **Repository:** GitHub
