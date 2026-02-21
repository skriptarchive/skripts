### MineMeeerMC Perks – Skript Description

This Skript provides a fully functional **perk system** for Minecraft servers, including a GUI menu, toggleable perks, voucher redemption, and admin management commands. It is designed to work with **LuckPerms** for permission handling and **Oraxen** for custom voucher items.

### ✨ Features

**Perk GUI**

- Opens with `/perks` (or `/perk`, `/mmperk`, `/mmp`, `/mmperks`)

- Displays available perks in a chest menu

- Shows locked perks with barriers and lore if the player does not have permission

- Allows players to toggle their perks on/off with a single click

**Available Perks**

- **Fly Perk**  
  Allows players to enable or disable flight.  
  Flight status is saved and automatically applied when the player joins.

- **Keep Inventory Perk**  
  Prevents item drops on death when enabled.  
  Players keep their inventory and receive a confirmation message.

- (new perks will be added in newer versions)

**Voucher System**

- Uses **Oraxen items** as redeemable perk vouchers

- Right-clicking a voucher opens a confirmation GUI

- Players can:
  
  - Redeem the voucher → automatically grants the LuckPerms permission
  
  - Cancel the process

- Prevents redeeming a perk the player already owns

**Admin Commands**

- `/perk ver` → Shows the current perk script version

- `/perk reload` → Reloads the Skript file

- `/perk give <player> <perk> [amount]` → Gives Fly or KeepInventory vouchers

- Permission-based access for all admin functions

**Permissions**

- `mmperks.cmd` → Base command access

- `mmperks.gui` → Open perk GUI

- `mmperks.fly` → Use Fly perk

- `mmperks.keepinv` → Use KeepInventory perk

- `mmperks.admin` / `mmperks.reload` → Admin controls

### Persistence

- Perk states are stored in variables and restored on player join

- Fly is automatically re-enabled if it was active before logout

### Integration

- **LuckPerms** → Grants permanent perk permissions when redeeming vouchers

- **Oraxen** → Provides custom voucher items

### Safety & UX

- GUI clicks are fully cancelled to prevent item stealing

- Clear success/error messages with a configurable prefix

- Locked perks display how to obtain them (e.g., crates)

## Configuration

The Main Configration is at the top and can be changed easily:

```yml
options:
    perk-menu-rows: 3
    menu-name: "MineMeeerMC-Perks"
    script-path: "/perks.sk"
    redeem-perk-rows: 1
    prefix: "&a&lMineMeeerPerks &r&8-&b "
    name: "MineMeeer Perks"
    primary: "&b"
    secundary: "&a"
    fly-perk-voucher: oraxen item "fly_perk_perma_voucher"
    keep-perk-voucher: oraxen item "keep_perk_perma_voucher"
```

## Code

The Code can be downloaded on the [**SkriptArchive Skript GitHub**](https://github.com/skriptarchive/skripts/tree/main/Utility/mmperks) and can be copied here:

```yml
options:
    perk-menu-rows: 3
    menu-name: "MineMeeerMC-Perks"
    script-path: "/perks.sk"
    redeem-perk-rows: 1
    prefix: "&a&lMineMeeerPerks &r&8-&b "
    name: "MineMeeer Perks"
    primary: "&b"
    secundary: "&a"
    fly-perk-voucher: oraxen item "fly_perk_perma_voucher"
    keep-perk-voucher: oraxen item "keep_perk_perma_voucher"

command /perks [<text>] [<player>] [<text>] [<number>]:
    aliases: /perk, /mmperk, /mmp, /mmperks
    permission: mmperks.cmd
    permission message: &cYou don't have permission for that!
    trigger:
        if arg 1 is not set:
            if player has permission "mmperks.gui":
                openPerkGUI(player)

        else if arg 1 is "ver":
            if player has permission "mmperks.admin":
                send {@prefix} + {@primary} + "This server is running " + {@secundary} + "MineMeeerMC-Perks" + {@primary} + " version 0.1!"

        else if arg 1 is "fly":
            if player has permission "mmperks.fly":
                if {fly_perk_enabled::%player%} is not true:
                    allow flight to event-player
                    set {fly_perk_enabled::%player%} to true
                    send {@prefix} + "" + {@secundary} + "Fly-Perk" + {@primary} + " was successfully activated."
                else if {fly_perk_enabled::%player%} is true:
                    disallow flight to event-player
                    set {fly_perk_enabled::%player%} to false
                    send {@prefix} + "&cFly-Perk" + {@primary} + " was successfully deactivated."
        else if arg 1 is "keepinv":
            if player has permission "mmperks.keepinv":
                if {keepinv_perk_enabled::%player%} is not true:
                    set {keepinv_perk_enabled::%player%} to true
                    send {@prefix} + "" + {@secundary} + "Keep-Inventory-Perk" + {@primary} + " was successfully activated."
                else if {keepinv_perk_enabled::%player%} is true:
                    set {keepinv_perk_enabled::%player%} to false
                    send {@prefix} + "&cKeep-Inventory-Perk" + {@primary} + " was successfully deactivated."
            else:
                send {@prefix} + "&cYou don't have permission for that!"
            stop

        else if arg 1 is "halloo":
            if player has permission "command.halloo":
                send {@prefix} + "" + {@secundary} + "HaHello"
            else:
                send {@prefix} + "&cYou don't have permission for that!"
            stop
        else if arg 1 is "reload":
            if player has permission "mmperks.reload":
                send {@prefix} + "" + {@secundary} + "&l" + {@name} + "&r" + {@primary} + " is being reloaded..."
                reload script {@script-path}
            else:
                if player has permission "mmperks.admin":
                    send {@prefix} + "" + {@secundary} + "&l" + {@name} + "&r" + {@primary} + " is being reloaded..."
                    reload script {@script-path}
                else:
                    send {@prefix} + "&cYou don't have permission for that!"
        else if arg 1 is "give":
            if player has permission "mmperks.admin":
                if arg 2 is not set:
                    send {@prefix} + "" + {@primary} + "Correct usage: " + {@secundary} + "/perk give <player> <perk> [amount]"
                if arg 4 is number:
                    set {_amount} to arg 4
                    if arg 2 is not set:
                        send {@prefix} + "" + {@primary} + "Correct usage: " + {@secundary} + "/perk give <player> <perk> <amount>"
                    else:
                        set {_giveto} to arg 2
                        if arg 3 is not set:
                            send {@prefix} + "" + {@primary} + "I know these perks: " + {@secundary} + "fly" + {@primary} + ", " + {@secundary} + "keep"
                        if arg 3 is not "fly" or "keep":
                            send {@prefix} + "" + {@primary} + "I only know these perks: " + {@secundary} + "fly" + {@primary} + ", " + {@secundary} + "keep"
                        if arg 3 is "fly":
                            give {_giveto} {_amount} of {@fly-perk-voucher}
                        else if arg 3 is "keep":
                            give {_giveto} {_amount} of {@keep-perk-voucher}
                else if arg 4 is not set:
                    set {_amount} to 1
                    if arg 2 is not set:
                        send {@prefix} + "" + {@primary} + "Correct usage: " + {@secundary} + "/perk give <player> <perk> <amount>"
                    else:
                        set {_giveto} to arg 2
                        if arg 3 is not set:
                            send {@prefix} + "" + {@primary} + "I know these perks: " + {@secundary} + "fly" + {@primary} + ", " + {@secundary} + "keep"
                        if arg 3 is not "fly" or "keep":
                            send {@prefix} + "" + {@primary} + "I only know these perks: " + {@secundary} + "fly" + {@primary} + ", " + {@secundary} + "keep"
                        if arg 3 is "fly":
                            give {_giveto} {_amount} of {@fly-perk-voucher}
                        else if arg 3 is "keep":
                            give {_giveto} {_amount} of {@keep-perk-voucher}
                else:
                    send {@prefix} + "" + {@primary} + "Correct usage: " + {@secundary} + "/perk give <player> <perk> <amount>"

            else:
                send {@prefix} + "" + {@primary} + "The entered command is not (yet) supported by " + {@secundary} + "&l" + {@name} + "&r" + {@primary} + "."

        else:
            send {@prefix} + "" + {@primary} + "The entered command is not (yet) supported by " + {@secundary} + "&l" + {@name} + "&r" + {@primary} + "."

on inventory click:
    if name of event-inventory is {@menu-name}:
        cancel event

        # Fly
        if event-item is elytra named "&c&lFly-Perk":
            allow flight to event-player
            set {fly_perk_enabled::%player%} to true
            set slot 10 of player's current inventory to elytra named "" + {@secundary} + "&lFly-Perk"
        else:
            if event-item is elytra named "" + {@secundary} + "&lFly-Perk":
                disallow flight to event-player
                set {fly_perk_enabled::%player%} to false
                set slot 10 of player's current inventory to elytra named "&c&lFly-Perk"

        # Keep Inv
        if event-item is recovery_compass named "&c&lKeep-Inventory-Perk":
            set {keepinv_perk_enabled::%player%} to true
            set slot 11 of player's current inventory to recovery_compass named "" + {@secundary} + "&lKeep-Inventory-Perk"
        else:
            if event-item is recovery_compass named "" + {@secundary} + "&lKeep-Inventory-Perk":
                set {keepinv_perk_enabled::%player%} to false
                set slot 11 of player's current inventory to recovery_compass named "&c&lKeep-Inventory-Perk"


function toggleKeepInv(c: player, p: player):
# Keep
    if {keepinv_perk_enabled::%{_p}%} is not true:
        set {keepinv_perk_enabled::%{_p}%} to true
    else:
        if {keepinv_perk_enabled::%{_p}%} is true:
            set {keepinv_perk_enabled::%{_p}%} to false

function openPerkGUI(p: player):
    open chest inventory with {@perk-menu-rows} rows named {@menu-name} to {_p}
    if {_p} has permission "mmperks.fly":
        if {fly_perk_enabled::%{_p}%} is not true:
            set slot 10 of {_p}'s current inventory to elytra named "&c&lFly-Perk"
        if {fly_perk_enabled::%{_p}%} is true:
            set slot 10 of {_p}'s current inventory to elytra named "" + {@secundary} + "&lFly-Perk"
    else:
        set slot 10 of {_p}'s current inventory to barrier named "&c&lFly-Perk &r&c(not available)"
        set lore of slot 10 of {_p}'s current inventory to "" + {@primary} + "Get this perk from the " + {@secundary} + "&lVote-Crate&r" + {@primary} + "."


    if {_p} has permission "mmperks.keepinv":
        if {keepinv_perk_enabled::%{_p}%} is not true:
            set slot 11 of {_p}'s current inventory to recovery_compass named "&c&lKeep-Inventory-Perk"
        if {keepinv_perk_enabled::%{_p}%} is true:
            set slot 11 of {_p}'s current inventory to recovery_compass named "" + {@secundary} + "&lKeep-Inventory-Perk"
    else:
        set slot 11 of {_p}'s current inventory to barrier named "&c&lKeep-Inventory-Perk &r&c(not available)"
        set lore of slot 11 of {_p}'s current inventory to "" + {@primary} + "Get this perk from the " + {@secundary} + "&l3rd Crate&r" + {@primary} + "."


# Keepinv Code:
on death of a player:
    if {keepinv_perk_enabled::%victim%} is true:
        cancel the item drops
        keep the inventory
        send {@prefix} + "" + {@primary} + "Your " + {@secundary} + "&lKeep-Inventory-Perk" + {@primary} + " saved you from losing your items upon death!" to victim

on join:
    if player has permission "mmperks.cmd":
        if {fly_perk_enabled::%player%} is true:
            allow flight to player
            message "" + {@primary} + "Your perk has been applied!"
        else:
            send {@prefix} + "" + {@primary} + "You have no perks activated!"
        message "" + {@primary} + "Your perks have been applied!"








# Perk Vouchers

on right click:
    if player's tool is {@fly-perk-voucher}:
        cancel event
        if player has permission "mmperks.fly":
            send {@prefix} + "You cannot redeem this perk because you already have it!" to player
        else:
            open chest inventory with {@redeem-perk-rows} rows named {@menu-name} + " Fly-Redeem" to player
            set slot 2 of player's current inventory to lime_wool named "" + {@secundary} + "&lRedeem Voucher"
            set slot 4 of player's current inventory to {@fly-perk-voucher}
            set slot 6 of player's current inventory to red_wool named "&c&lCancel"

on inventory click:
    if name of event-inventory is {@menu-name} + " Fly-Redeem":
        cancel event
        if event-item is {@fly-perk-voucher}:
            close player's inventory
            send {@prefix} + "That is the item you want to redeem. Clicking it in the GUI does nothing."
        if event-item is lime_wool named "" + {@secundary} + "&lRedeem Voucher":
            close player's inventory
            execute server command "msg %player% wait a moment, I'm redeeming your voucher"
            remove 1 of {@fly-perk-voucher} from player's inventory
            execute server command "lp user %player% permission set mmperks.fly true"
            send {@prefix} + "You have successfully redeemed the voucher!" to player
            execute server command "msg %player% I have redeemed your voucher for you."
        if event-item is red_wool named "&c&lCancel":
            close player's inventory
            send {@prefix} + "Process was successfully cancelled."


on right click:
    if player's tool is {@keep-perk-voucher}:
        cancel event
        if player has permission "mmperks.keepinv":
            send {@prefix} + "You cannot redeem this perk because you already have it!" to player
        else:
            open chest inventory with {@redeem-perk-rows} rows named {@menu-name} + " Keep-Redeem" to player
            set slot 2 of player's current inventory to lime_wool named "" + {@secundary} + "&lRedeem Voucher"
            set slot 4 of player's current inventory to {@keep-perk-voucher}
            set slot 6 of player's current inventory to red_wool named "&c&lCancel"

on inventory click:
    if name of event-inventory is {@menu-name} + " Keep-Redeem":
        cancel event
        if event-item is {@keep-perk-voucher}:
            close player's inventory
            send {@prefix} + "That is the item you want to redeem. Clicking it in the GUI does nothing."
        if event-item is lime_wool named "" + {@secundary} + "&lRedeem Voucher":
            close player's inventory
            execute server command "msg %player% wait a moment, I'm redeeming your voucher"
            remove 1 of {@keep-perk-voucher} from player's inventory
            execute server command "lp user %player% permission set mmperks.keepinv true"
            send {@prefix} + "You have successfully redeemed the voucher!" to player
            execute server command "msg %player% I have redeemed your voucher for you."
        if event-item is red_wool named "&c&lCancel":
            close player's inventory
            send {@prefix} + "Process was successfully cancelled."
```
