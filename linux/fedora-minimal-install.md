# Fedora Minimal Install

These are my notes for a Fedora Minimal Install. The steps below are for execution on a Linux machine, but notes for other operating systems like Windows will be appended where appropriate.

## Writing to a Removable Drive

Start with a Fedora NetInstall image. Write the image to a blank drive. In this case, we are installing Fedora 30, but replace it with other versions depending on the release and your needs. Remember to make sure that you have absolutely no personal documents in that drive that you are going to install, as the steps in this section will completely wipe your drive clean!

```bash
# cd to relevant directory
curl -L https://download.fedoraproject.org/pub/fedora/linux/releases/30/Workstation/x86_64/iso/Fedora-Workstation-netinst-x86_64-30-1.2.iso
# do some verification (which is recommended)
dd if=Fedora-Workstation-netinst-x86_64-30-1.2.iso of=/dev/sdX status=progress
```

You could use other tools, like **Rufus** if you are on Windows.

For myself, I prefer shortcuts. I could lump the above steps (without verification) by doing the following instead (which of course from a security standpoint is bad):

```bash
curl -L https://download.fedoraproject.org/pub/fedora/linux/releases/30/Workstation/x86_64/iso/Fedora-Workstation-netinst-x86_64-30-1.2.iso \
 | dd of=/dev/sdX status=progress
```

Replace `sdX` with the appropriate location of the drive.

## Installing Fedora

Boot Fedora NetInstall from your removable media, then follow the steps until you get to the **Installation Summary**.

Under **Software Selection**, you are going to choose **Minimal Install**. You may check **Standard**, which provides a set of useful packages for your Fedora setup.

After setting 

Once finished, click **Reboot**.

## After Install

Boot up to your Fedora on the drive you have installed. You will be presented with a command line screen.

Check:

- Number of packages installed (~500)
- Number of background processes (~90)

```bash
rpm -qa | wc -l
ps aux | wc -l
```

Install X11 first:

```bash
sudo dnf install @base-x
```

Then install your preferred desktop environment, for example, Gnome:

```bash
sudo dnf install gnome-shell
```

Wifi is not auto configured on a Minimal Install, so depending on the Wifi card that you have installed, you can run a check using `lspci`, then install the relevant drivers. Most machines will have Intel Wifi cards installed, and the relevant packages are the `iwl` series:

```bash
sudo dnf install iwl*
```

If you are intending to use Fedora on just one machine and you know which Wifi card you have installed, you can include the model number by replacing `iwl*` with `iwl3160*` for example.

Once you have completed the above installation steps, make sure to switch over to the graphical interface:

```bash
sudo systemctl set-default graphical.target
```

...and then do a reboot:

```bash
sudo systemctl reboot
```

Then configure the rest of your Fedora disto to your hearts content!

I will commit the install scripts onto a separate repo.

## Acknowlegements

- https://www.reddit.com/r/Fedora/comments/a6c60d/my_notes_on_a_minimal_desktop_install_of_fedora_29/
