# Keybinds

Here is a quick guid on how to add keybinds to your hacked client. <br>
This tutorial is intended for 1.19 clients, but it should work for later versions

## Step 1

Inside of `CLIENTNAME\module\settings`, create a new class called `KeyBindSetting.java` <br>
In the `KeyBindSetting.java` class, put in the following code:

```java
package CLIENTNAME.module.settings;

public class KeyBindSetting extends Setting {
    
    private int key;
	private boolean enabled;
    
    public KeyBindSetting(String name, int defaultKey) {
        super(name);
        this.key = defaultKey;
    }
    public int getKey() {
    	return key;
    }

    public void setKey(int key) {
    	this.key = key;
    }

    public void toggle() {
    	this.enabled = !this.enabled;
    }
}
```

## Step 2
Inside of `CLIENTNAME\ui\screens\clickGUI\setting;` create a new class called `KeyBind.java` and paste the following code inside:

```java
package CLIENTNAME.ui.screens.clickGUI.setting;

import java.awt.Color;

import net.minecraft.client.font.TextRenderer;
import net.minecraft.client.gui.DrawableHelper;
import net.minecraft.client.util.math.MatrixStack;
import net.minecraft.util.Identifier;
import CLIENTNAME.module.settings.KeyBindSetting;
import CLIENTNAME.module.settings.Setting;
import CLIENTNAME.ui.screens.clickGUI.ModuleButton;

public class KeyBind extends Component {

	private KeyBindSetting binding = (KeyBindSetting)setting;
	public boolean isBinding = false;
	
	public KeyBind(Setting setting, ModuleButton parent, int offset) {
		super(setting, parent, offset);
	}

	@Override
	public void mouseClicked(double mouseX, double mouseY, int button) {
		if (isHovered(mouseX, mouseY) && button == 0) {
			binding.toggle();
			isBinding = true;
		}
		super.mouseClicked(mouseX, mouseY, button);
	}
	
	@Override
	public void keyPressed(int key) {
		if (isBinding == true) {
			parent.module.setKey(key);
			binding.setKey(key);
			isBinding = false;
		}
		if ((binding.getKey() == 256)) {
			parent.module.setKey(0);
			binding.setKey(0);
			isBinding = false;
		}
		if ((binding.getKey() == 259)) {
			parent.module.setKey(0);
			binding.setKey(0);
			isBinding = false;
		}
		super.keyPressed(key);
	}

	@Override
	public void render(MatrixStack matrices, int mouseX, int mouseY, float delta) {
		
		DrawableHelper.fill(matrices, parent.parent.x, parent.parent.y + parent.offset + offset, parent.parent.x + parent.parent.width, parent.parent.y + parent.offset + offset + parent.parent.height, 0x90000000);

		int offsetY = ((parent.parent.height / 2) - mc.textRenderer.fontHeight / 2);
		
		if (isBinding==false) mc.textRenderer.drawWithShadow(matrices, "Keybind: " + binding.getKey(), parent.parent.x + offsetY, parent.parent.y + parent.offset + offset + offsetY-6, -1);
		if (isBinding==true) mc.textRenderer.drawWithShadow(matrices, "Binding...", parent.parent.x + offsetY, parent.parent.y + parent.offset + offset + offsetY-6, -1);

		
		super.render(matrices, mouseX, mouseY, delta);
	}
}
```

## Step 3

Inside of your `ModuleButton.java` class located inside of `CLIENTNAME\ui\screens\clickGUI\setting;`<br>
Locate the following text (should be arround line 55)
<br><br>
![Image](https://cdn.discordapp.com/attachments/684812018099814472/1018493054795128893/unknown.png)
<br> and add the following code:
```java
 else if (setting instanceof KeyBindSetting) {
				components.add(new KeyBind(setting, this, setOffset));
			}
```
Then, at then end of the `ModuleButton.java` class add the following code:
```java
	public void keyPressed(int key) {
        for (Component component : components) {
            component.keyPressed(key);
        }
    }
```
## Step 4

Inside of the `Mod.java` class located inside of `CLIENTNAME\module`, add the folowing code to match the screenshot
```java
addSetting(new KeyBindSetting("Key", 0));
```
<br>
![Image](https://cdn.discordapp.com/attachments/1012433630666166292/1012836510862692512/IMG_0582.png)

## Step 5

Inside of your `Frame.java` class (`CLIENTNAME\ui\screens\clickGUI`) add the following code:
```java
public void keyPressed(int key) {
        for (ModuleButton mb : buttons) {
            mb.keyPressed(key);
}
```

Then at the end of your `clickGUI.java` class (`CLIENTNAME\ui\screens\clickGUI`) add the following code:
```java
@Override
public boolean keyPressed(int keyCode, int scanCode, int modifiers) {
    for (Frame frame : frames) {
        frame.keyPressed(keyCode);
    }
    return super.keyPressed(keyCode, scanCode, modifiers);
}
```

# Enjoy!

If you followed all the steps correctly, every modules should now have a setting called `Keybind: 0`. 
<br>
verything works, should should be able to simply click on the setting to set a keybind!
<br><br>
Need some help? Contact me on
[Discord](https://discord.gg/jBHTMgEXXk)!
