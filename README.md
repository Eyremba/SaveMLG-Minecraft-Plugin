# SaveMLG-Minecraft-Plugin


package me.PhilMc.SaveMLG;

import java.util.ArrayList;

import org.bukkit.Location;
import org.bukkit.Material;
import org.bukkit.World;
import org.bukkit.block.Block;
import org.bukkit.command.ConsoleCommandSender;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.entity.EntityDamageEvent;
import org.bukkit.event.entity.EntityDamageEvent.DamageCause;
import org.bukkit.plugin.java.JavaPlugin;

public class main extends JavaPlugin implements Listener{
	
	ConsoleCommandSender console = getServer().getConsoleSender();
	
	@Override
	public void onEnable() {
		console.sendMessage("[SaveMLG] Enabled");
		getServer().getPluginManager().registerEvents(this, this);	
	}
	public void onDisable() {
		console.sendMessage("[SaveMLG] Disabled");
	}
	
	
	public ArrayList<Block> getBlocksBelow(Player player) {
	    ArrayList<Block> blocksBelow = new ArrayList<Block>();
	    Location location = player.getLocation();
	    double x = location.getX();
	    double z = location.getZ();
	    World world = player.getWorld();
	    double yBelow = player.getLocation().getY();
	    double yBelow2 = player.getLocation().getY() + 1;
	    Block northEast = new Location(world, x + 0.3, yBelow, z - 0.3).getBlock();
	    Block northWest = new Location(world, x - 0.3, yBelow, z - 0.3).getBlock();
	    Block southEast = new Location(world, x + 0.3, yBelow, z + 0.3).getBlock();
	    Block southWest = new Location(world, x - 0.3, yBelow, z + 0.3).getBlock();
	    Block northEast2 = new Location(world, x + 0.3, yBelow2, z - 0.3).getBlock();
	    Block northWest2 = new Location(world, x - 0.3, yBelow2, z - 0.3).getBlock();
	    Block southEast2 = new Location(world, x + 0.3, yBelow2, z + 0.3).getBlock();
	    Block southWest2 = new Location(world, x - 0.3, yBelow2, z + 0.3).getBlock();
	    Block[] blocks = {northEast, northWest, southEast, southWest, northEast2, northWest2, southEast2, southWest2};
	    for (Block block : blocks) {
	        if (!blocksBelow.isEmpty()) {
	            boolean duplicateExists = false;
	            for (int i = 0; i < blocksBelow.size(); i++) {
	                if (blocksBelow.get(i).equals(block)) {
	                    duplicateExists = true;
	                }
	            }
	            if (!duplicateExists) {
	                blocksBelow.add(block);
	            }
	        } else {
	            blocksBelow.add(block);
	        }
	    }
	    return blocksBelow;
	}
	
	
	@EventHandler
	public void onDamage (EntityDamageEvent e) {
		if (e.getEntity() instanceof Player) {
			Player p = (Player) e.getEntity();
			if (e.getCause() == DamageCause.FALL) {
				ArrayList<Block> blocks = getBlocksBelow(p);
				for (Block block : blocks) {
					if (block.getType().equals(Material.WEB)) e.setCancelled(true);
				}
			}
		}
	}
}
