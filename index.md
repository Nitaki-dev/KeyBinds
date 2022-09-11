# Keybinds

Here is a quick guid on how to add keybinds to your hacked client. <br>
This tutorial is intended for 1.19 clients, but it should work for later versions

## Step 1

Inside of `src\main\java\CLIENTNAME\module\settings`, create a new class called `KeyBindSetting.java` <br>
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


**YES** _no_ `maybe` haha

Need some help? Contact me on
[ Discord!](https://discord.gg/jBHTMgEXXk)
