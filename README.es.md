# ¿Cómo usar este repositorio?

Mientras la PR del paquete sigue en espera (https://github.com/NixOS/nixpkgs/pull/405382), puedes usar este repositorio como un overlay para tu configuración de NixOS:

Fichero overlay `./overlays/libpkcs11-dnie.nix`:
```nix
self: super: {
  libpkcs11-dnie = super.callPackage (super.fetchFromGitHub {
    owner = "alexland7219";
    repo = "libpkcs11-dnie-nix";
    rev = "v1.6.8-1";
    sha256 = "sha256-AwyyC5EOYbte+/rpRzZTyTS2VSGA5GKdKNa2vQhoFIc=";
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
