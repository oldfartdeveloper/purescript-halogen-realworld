# Scott's Experience Installing Everything

My deviations from using the README in this folder are documented here.

## Installation

I cloned the repo as instructed

1. First, clone the repository:

   ```sh
   git clone https://github.com/thomashoneyman/purescript-halogen-realworld
   cd purescript-halogen-realworld
   ```

1. But, in order to do `nix shell`, I had to install **nix**.  This I did
   using a **[script](https://medium.com/@robinbb/install-nix-on-macos-catalina-ca8c03a225fc)
   which I repeat as follows:

   ```sh
   echo 'nix' | sudo tee -a /etc/synthetic.conf
   reboot  # Actually reboot your Mac.
   sudo diskutil apfs addVolume disk1 APFSX Nix -mountpoint /nix
   sudo diskutil enableOwnership /nix
   sudo chflags hidden /nix
   echo "LABEL=Nix /nix apfs rw" | sudo tee -a /etc/fstab
   sh <(curl https://nixos.org/nix/install) --daemon 
   ```

   The above worked until the last step; that didn't actually install anything.

   But what did work was that the volume, `/nix`, was created in the iMac's SSD drive.
   So I cd'd into that location (`/nix`), and reran the last command
   (`sh <(curl https://nixos.org/nix/install) --daemon`), and this succeeded.

   In addition, I verified the installation w/ the command
   [from here](https://nixos.org/guides/install-nix.html):

   ```sh
   $ nix-env --version
   nix-env (Nix) 2.3.6
   ```

1. I then went back to this project's directory and entered:

   ```sh
   nix-shell
   ```

   which put me into a nix shell whose prompt is:

   ```nix
   [nix-shell:~/projects/purescript-halogen-realworld]
   ```

   and it then
   installed everything I need (except JavaScript dependencies) into this project.

1. I then installed JavaScript dependencies:

   ```nix
   npm install
   ```

## Building and running

This section ran exactly as documented.

## Dev mode

There are two modes:

1. One for watching:

   ```sh
   npm run watch
   ```

1. The other for running the application in the server:

   ```sh
   npm run serve-dev
   ```

When I ran `npm run watch`, it was not running in a daemon and I couldn't execute
the other (`npm run serve-dev`).  So I guessed that if I started up another terminal
in VS Code and launched `nix-shell` followed by the 2nd command (`npm run serve-dev`),
that it would:

1. Run it in a separate process.
1. Link to the nix processes already installed for the project (i.e not install a copy).

And indeed it did.  I have a development environment for PureScript running under `nix` now,
and I appreciate the additional `nix` features.
