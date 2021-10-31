# Docs

This Git repository is a place for all documents subject to the version control, i.e. API specs etc.

The [Wiki](https://github.com/zero-air/docs/wiki) on this repository is a Knowledge Base, i.e. guides, F.A.Q. etc.

# Goals

Learn! This project started by @dee-kryvenko for @dee-kryvenko exclusively to boost his knowledge including but not limited to:

- Soldering
- PCB
- Raspberry Pi (and potentially Arduino)
- System programming with Go
- mDNS/DNS-SD
- ZeroConf with Go
- Prometheus and Grafana in Kubernetes
- GitHub Actions, Environments and Project Management
- Potentially also: Consul and Vault in Kubernetes

Official excuse to allocate funds for this project from a family budget was: we need to monitor air quality in our apartment! Hence the project name - Zero Air, which is per [Wiki](https://en.wiktionary.org/wiki/zero_air):

> atmospheric air specially cleaned so that it contains less than 0.1 ppm of hydrocarbon impurities

Yeah yeah, I know, it is not what we breath - but such a cool name, isn't it?

# Approach

Ok, you are warned - the approach below is an outrageous overkill. After reviewing some other projects in this space - I have identified my primary inspiration: [YouTube: Your home's air could be making you sick. Fight back!](https://www.youtube.com/watch?v=Cmr5VNALRAg). As you can see from that video, building an air quality monitor can be much cheaper, simpler and faster than what I am going to do here. Some additional inspirational videos:

- [DIY Air Quality Monitor - PM2.5, CO2, VOC, Ozone, Temp & Hum Arduino Meter](https://www.youtube.com/watch?v=esY_OtDLv7g&t=9s)
- [Raspberry pi based indoor air quality environmental monitor](https://www.youtube.com/watch?v=nV4-rTF3aOk)
- [How to build an air quality monitor using Raspberry Pi Zero W + ANAVI Infrared pHAT](https://www.youtube.com/watch?v=0gNnUWtbmeI)

But - look again at my goals! If I just wanted to monitor the air quality - I would have bought a commercial product. So, don't even try to question "why" when you read below - there is no rationale and most of the time the answer going to be "because I can (or want to believe I can)".

My occupation is in software engineering - mostly around clouds, kubernetes, infra as code and CI/CD. So, there will be k8s 100% - as well as desire to bring Prometheus, Grafana, Consul and Vault into that environment as I want to improve my expertise with these awesome tools and dig much deeper than I ever had to previously. Also - I have a huge gap with mDNS/DNS-SD that I wanted to breach as part of this project. Finally, it is my third year developing with Go and a some extra practice always welcome - especially when it comes to system programming such as reading hardware stats and ZeroConf discovery. And this is how I've came up with the software stack for this project.

When it comes to hardware - I am terrible at soldering and I have next to zero understanding how circuit boards actually works. I'm also sitting so high up in the abstractions that the low level system programming feels like a rocket science to me. But - I want to learn. And I thought it'd be easier to start with something like Raspberry Pi with a fully fledged Linux onboard with my favorite Go or at least some Python at my disposal (rather than something like Arduino that I'd have to flash a firmware and write low level system code).

With that, the preliminary high level project design goes like that:

1. Raspberry Pi Zero (or Zero 2) W with a bunch of sensors (CO2, particulate matter, temperature, humidity at the very least) advertising themselves via mDNS/DNS-SD and exporting data from sensors via the prometheus endpoint.
2. Prometheus lives in K8s and scraping data from what it can discover with ZeroConf on the local network.
3. Grafana lives in K8s and used to visualize data in Prometheus.
4. Finally - a Raspberry Pi 4 in a case with embedded screen sitting on the desk or hanging on the wall and displaying data from Grafana (as well as possibly running aforementioned K8s cluster).
