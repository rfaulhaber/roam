# nix flakes

Nix flakes are a type of package for [[NixOS]]. They are &ldquo;self-contained units that have inputs (dependencies) and outputs (packages).&rdquo; They are written in the [[Nix]] language.


## Structure

A flake is a directory that simply contains a `flake.nix` file. Nix flakes are somewhat simple: they are Nix objects with an input and an output.

```nix
{
  outputs = { self }: {
    # some arbitrary output
    # `nix eval .#foo` would output "bar"
    foo = "bar";
  };
}
```

Some outputs have more specific uses, however. Taken from the NixOS wiki:

```nix
{ self, ... }@inputs:
{
  # Executed by `nix flake check`
  checks."<system>"."<name>" = derivation;
  # Executed by `nix build .#<name>`
  packages."<system>"."<name>" = derivation;
  # Executed by `nix build .`
  # From nix >= 2.7, this is now 'packages.<system>.default'
  defaultPackage."<system>" = derivation;
  # Executed by `nix run .#<name>`
  apps."<system>"."<name>" = {
    type = "app";
    program = "<store-path>";
  };
  # Executed by `nix run . -- <args?>`
  # From nix >= 2.7, this is now 'apps.<system>.default'
  defaultApp."<system>" = { type = "app"; program = "..."; };

  # Used for nixpkgs packages, also accessible via `nix build .#<name>`
  legacyPackages."<system>"."<name>" = derivation;
  # Default overlay, consumed by other flakes
  overlay = final: prev: { };
  # Same idea as overlay but a list or attrset of them.
  overlays = {};
  # Default module, consumed by other flakes
  nixosModule = { config }: { options = {}; config = {}; };
  # Same idea as nixosModule but a list or attrset of them.
  nixosModules = {};
  # Used with `nixos-rebuild --flake .#<hostname>`
  # nixosConfigurations."<hostname>".config.system.build.toplevel must be a derivation
  nixosConfigurations."<hostname>" = {};
  # Used by `nix develop`
  # From nix >= 2.7, this is now 'devShells.<system>.default'
  devShell."<system>" = derivation;
  # Used by `nix develop .#<name>`
  devShells."<system>"."<name>" = derivation;
  # Hydra build jobs
  hydraJobs."<attr>"."<system>" = derivation;
  # Used by `nix flake init -t <flake>`
  defaultTemplate = {
    path = "<store-path>";
    description = "template description goes here?";
  };
  # Used by `nix flake init -t <flake>#<name>`
  templates."<name>" = { path = "<store-path>"; description = ""; };
}
```


## References

-   [Practical Nix Flakes](https://serokell.io/blog/practical-nix-flakes)
-   [Nix Flakes, Part 1: An introduction and tutorial - Tweag](https://www.tweag.io/blog/2020-05-25-flakes/)
-   [Nix Flakes, Part 2: Evaluation caching - Tweag](https://www.tweag.io/blog/2020-06-25-eval-cache/)
-   [Nix Flakes, Part 3: Managing NixOS systems - Tweag](https://www.tweag.io/blog/2020-07-31-nixos-flakes/)
-   [Flakes - NixOS Wiki](https://nixos.wiki/wiki/Flakes)

