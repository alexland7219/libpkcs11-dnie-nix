# ¿Cómo usar este repositorio?

Mientras la PR del paquete sigue en espera (https://github.com/NixOS/nixpkgs/pull/405382), puedes usar este repositorio como un overlay para tu configuración de NixOS:

Fichero overlay `./overlays/libpkcs11-dnie.nix`:
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

Añade esto a tu `configuration.nix`:

```nix
{

  nixpkgs.overlays = [
    (import ./overlays/libpkcs11-dnie.nix)
  ];

}
```

Después de regenerar tu configuración, se imprimirá en pantalla más información a seguir.
Si estás usando NixOS, la mejor manera para poder usar tu DNIe es añadiendo esto a tu `configuration.nix`:

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
