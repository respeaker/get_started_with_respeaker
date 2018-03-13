## Out-of-box Demo

This OOB demo shows how to build an AVS (Alexa Voice Service) device in least command line knocks.

### Step 1

Before we start, we need you to know the basic operations of this board:
- System image burning
- Get serial console via OTG USB port
- Setup Wi-Fi / ethernet
- SSH
- VNC

### Step 2

ssh to the board, execute the following command:

```shell
curl https://raw.githubusercontent.com/respeaker/respeakerd/master/scripts/install_all.sh|bash
```

When prompt, type in the sudo password for user `respeaker`: respeaker. Wait the script install some packages, then follow the instructions printed. When finished the script, run:

```shell
python /home/respeaker/respeakerd/clients/Python/demo_respeaker_v2_vep_alexa_with_light.py
```

Enjoy!

### Next

- [AVS Guide](/docs/ReSpeaker_Core_V2/avs_guide.md)