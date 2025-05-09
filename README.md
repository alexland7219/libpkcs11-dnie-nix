# How to use this repository?

📖 Este documento está disponible en [Español](README.es.md)

While the PR for the package waits (https://github.com/NixOS/nixpkgs/pull/405382), you can use this repo as an overlay for your NixOS configuration:

Overlay file `./overlays/libpkcs11-dnie.nix`:
```nix
self: super: {
  libpkcs11-dnie = super.callPackage (super.fetchFromGitHub {
    owner = "alexland7219";
    repo = "libpkcs11-dnie-nix";
    rev = "5647a0b6dfe350d83af39f4169a72e4ef127c77b";
    sha256 = "sha256-SQ60rq5gmr87lUV8blwk3S0H1JkNX6Q2j1MjSeLD0Fo=";
  } + "/package.nix") {};
}
```

Add this to your `configuration.nix`:

```nix
{

  nixpkgs.overlays = [
    (import ./overlays/libpkcs11-dnie.nix)
  ];

}
```
After rebuilding, more instructions will be printed on the terminal for you follow.
If you are using NixOS, the best way to get going with your DNIe is by adding this to your `configuration.nix`:

```nix
{

  services.pcscd.enable = true;

  programs.firefox = {
    enable = true;
    policies = {
      SecurityDevices = {
        Add = {
          "PKCS#11 DNIe" = "${pkgs.libpkcs11-dnie}/lib/libpkcs11-dnie.so";
        };
      };
    };
  };

}
```
