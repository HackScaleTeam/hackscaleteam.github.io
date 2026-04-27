---
layout: post
title: "Networking Path Series"
date: 2026-04-22
lang: en
category: networking-series
---
# Networking Path Series — Network connectivity.
Third lesson, where we will explore how data travels between nodes(devices)!

## Learning Objectives:

- How does data flow globally

- Internet speed measuring

---

## Network connection:

When data travels between networks, it can travel in many forms. The main idea for all forms of networks is sending bits(a bunch of 0s and 1s) and bytes(a byte is 8 bits); these bits and bytes can travel between networks in many ways:

1-Electrical Signals, they travel in copper cables, your NIC(Network interface card) converts data into voltage changes, and these voltage changes travel throughout UTP (unshielded twisted pairs) cables. 

<p align="center">
  <img src="/assets/network_cable.png" width="400">
</p>

---

2- Radio Waves (Wi-Fi, Bluetooth): your data gets converted into electromagnetic waves, which are transmitted via antennas throughout the air.

<p align="center">
  <img src="/assets/giant_satellite.png" width="600">
</p>

---

3- Light signals (Fiber optic): Your data gets converted into light signals (pulses), and the light will travel through the fiber glass of the fiber optic cable. Fiber is the most favored method cause it’s fast (as fast as almost the speed of light), and it provides long-distance transmission with low losses.

<p align="center">
  <img src="/assets/fiber_cable.png" width="600">
</p>

---
We measure data flow with what we call (bps) bits per second. A bit is a value of ‘0’ or ‘1’. A byte is 8 bits, as we have mentioned above.

1 kilobit (Kb) = 1000 bits

1 megabit(Mb) = 1,000,000 bits

1 gigabit(Gb) = 1,000,000,000 bits

1 terabit (Tb) = 1,000,000,000,000 bits

Now we will talk about the standards and protocols that we use to make these technologies actually work! Mainly Ethernet, but what’s Ethernet?

Ethernet is a collection of network protocols/standards, and the benefits of using these protocols and standards are:

•	It provides the communication standards necessary for communications over the network.

•	It provides hardware standards so network hardware devices can communicate with each other.

Credit for all this hard work goes to two institutions:

•	The Institute of Electrical and Electronics Engineers (IEEE) and the IETF.

•	The Internet Engineering Task Force (IETF).



