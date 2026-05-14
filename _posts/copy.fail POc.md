# Copy Fail (CVE-2026-31431) — Linux Kernel LPE PoC

Welcome to another technical blog post from the HackScale team.

Today, we will demonstrate the famous Copy Fail vulnerability (CVE-2026-31431), a Linux kernel Local Privilege Escalation vulnerability discovered by Theori / Xint Code.

What makes this vulnerability dangerous is that the same exploit works across almost every major Linux distribution released since 2017 without requiring kernel-specific offsets or race conditions.

In this post, we will clone the official PoC repository and execute the exploit in a lab environment.

Let’s begin.

# Cloning the Repository

First, copy the official repository link from GitHub.

![clone](/assets/1.png)

Then clone the repository using the following command:

```bash
git clone https://github.com/theori-io/copy-fail-CVE-2026-31431
```

![git](/assets/2.png)

# Entering the Project Directory

After downloading the repository, move into the project directory:

```bash
cd copy-fail-CVE-2026-31431
```

To display the project files:

```bash
ls
```

![cd](/assets/3.png)

# Viewing the Exploit Code

The repository contains a small Python exploit called:

```bash
copy_fail_exp.py
```

Display the exploit source code using:

```bash
cat copy_fail_exp.py
```

![cat](/assets/4.png)

The exploit is extremely small and works using Linux kernel page cache corruption techniques.

# Running the Exploit

Before running the exploit, make sure you are using a vulnerable Linux kernel.

The exploit was tested successfully on multiple Linux distributions released after 2017.

Run the exploit using:

```bash
python3 copy_fail_exp.py
```

![run](/assets/5.png)

The exploit targets `/usr/bin/su` by default and attempts to modify the page cache of the binary in memory.

# Obtaining Root Access

After executing the exploit successfully, run:

```bash
su
```

If the system is vulnerable, you should obtain a root shell.

Verify the privileges using:

```bash
id
```

![root](/assets/6.png)

# Why This Vulnerability Is Dangerous

According to Theori, Copy Fail is a logic flaw inside the Linux kernel crypto subsystem.

The vulnerability involves:

* `authencesn`
* `AF_ALG`
* `splice()`
* Linux page cache corruption

An unprivileged local user can perform a controlled write into the page cache of readable files and abuse this behavior to gain root privileges.

One of the most dangerous aspects of this vulnerability is that the modification happens only in memory and may not appear in traditional file integrity monitoring solutions.

# Affected Systems

The vulnerability affects many Linux distributions released since 2017, including:

* Ubuntu
* Debian
* Kali Linux
* RHEL
* Amazon Linux
* SUSE
* Arch Linux
* Fedora

The impact depends on the running kernel version and whether the vulnerable crypto module is enabled.

# References

* https://github.com/theori-io/copy-fail-CVE-2026-31431
* https://copy.fail/
* https://www.helpnetsecurity.com/2026/04/30/copyfail-linux-lpe-vulnerability-cve-2026-31431/
* https://github.com/TheMalwareGuardian/CVE-2026-31431
* https://man7.org/linux/man-pages/man2/splice.2.html
* https://man7.org/linux/man-pages/man7/af_alg.7.html

# Conclusion

In this post, we demonstrated the Copy Fail (CVE-2026-31431) Linux kernel vulnerability and executed the public Proof of Concept released by Theori.

This vulnerability shows how dangerous logic flaws inside low-level kernel components can become, especially when they affect almost every modern Linux distribution for nearly a decade.

Always patch vulnerable kernels and avoid testing public exploits outside authorized lab environments.

Stay tuned for more technical write-ups and cybersecurity research from the HackScale team.
