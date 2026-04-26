package me.victor.rank;

import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.plugin.java.JavaPlugin;

public class Main extends JavaPlugin {

    @Override
    public void onEnable() {
        saveDefaultConfig();
        getLogger().info("RankPlugin pornit!");
    }

    @Override
    public void onDisable() {
        saveConfig();
        getLogger().info("RankPlugin oprit!");
    }

    public String getRank(String player) {
        return getConfig().getString("ranks." + player, "Player");
    }

    public void setRank(String player, String rank) {
        getConfig().set("ranks." + player, rank);
        saveConfig();
    }

    @Override
    public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {

        // /rank
        if (label.equalsIgnoreCase("rank")) {
            if (sender instanceof Player) {
                Player p = (Player) sender;
                String rank = getRank(p.getName());
                p.sendMessage("§aRank-ul tau este: §e" + rank);
            }
            return true;
        }

        // /setrank
        if (label.equalsIgnoreCase("setrank")) {

            if (!sender.hasPermission("rank.admin")) {
                sender.sendMessage("§cNu ai permisiune!");
                return true;
            }

            if (args.length < 2) {
                sender.sendMessage("§cFolosire: /setrank <jucator> <rank>");
                return true;
            }

            String player = args[0];
            String rank = args[1];

            setRank(player, rank);

            sender.sendMessage("§aRank setat!");
            return true;
        }

        return false;
    }
}
