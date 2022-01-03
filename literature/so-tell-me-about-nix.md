# So, tell me about Nix

-   [source](https://ghedam.at/15490/so-tell-me-about-nix)
-   [[Nix]] [[Linux]]


## Notes

-   Nix is:
    -   A package manager
    -   A programming language
    -   An operating system (NixOS, a Linux distro)
-   Nix can be used like `brew` or `apt`
    
    ```bash
      # this will install curl
      nix-env -iA curl
    ```
-   Nix&rsquo;s selling point is that it&rsquo;s capable of reproducible builds
-   One thing that&rsquo;s unique about Nix (the package manager) is that you can create ad-hoc development environments
    
    ```bash
      # this spawns a new shell with curl installed
      # once this shell is exited, it will no longer be available
      nix-shell -p curl
    ```
-   `nix-shell` allows for the creation of isolated development environments
-   NixOS, unlike other Linux distros, allows a user to specify their configuration using the Nix package manager
    -   For example, if I wanted to create a user whose default shell was `zsh`, I could do this via a configuration. The rest of my system&rsquo;s configuration would live here too
    -   This also allows for rollbacks to any previous configuration
    -   Nix, being a [[functional programming]] language, basically describes system configuration like `state = nextState(currentState)`


## Resources (from article)

-   [GitHub - rycee/home-manager: Manage a user environment using Nix](https://github.com/rycee/home-manager)
-   [GitHub - NixOS/nixops: NixOps is a tool for deploying to NixOS machines in a &#x2026;](https://github.com/NixOS/nixops)
-   [GitHub - LnL7/nix-darwin: nix modules for darwin](https://github.com/LnL7/nix-darwin)
-   [A tour of Nix](https://nixcloud.io/tour/)
-   [A Gentle Introduction to the Nix Family](https://ebzzry.io/en/nix/)
-   [NixOS - Nix Pills](https://nixos.org/nixos/nix-pills/)
