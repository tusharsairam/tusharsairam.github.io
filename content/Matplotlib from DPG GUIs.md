---
date: 2025-04-09 01:50
---
When a matplotlib plot is generated from a DPG thread, this warning often shows up

```
UserWarning: Starting a Matplotlib GUI outside of the main thread will likely fail
```

and the DPG window's contents shrink to a tiny size. Moreover, the DPG GUI fails to respond, causing a crash. Threading didn't work for me but using Python's `multiprocessing` library worked

Offload the matplotlib GUI into a separate *process* altogether. Here's the minimal code

```python
import dearpygui.dearpygui as dpg
import multiprocessing
import time

def run_matplotlib():
    # This function will run in a separate process
    import matplotlib.pyplot as plt
    plt.plot([1, 2, 3, 4, 5])
    plt.title("Matplotlib Plot")
    plt.xlabel("X-axis")
    plt.ylabel("Y-axis")
    plt.show()

def open_graph():
    process = multiprocessing.Process(target=run_matplotlib)
    process.daemon = True  # This ensures the process terminates when the main program exits
    process.start()

if __name__ == "__main__":
    dpg.create_context()
    dpg.create_viewport(title="DearPyGui with Matplotlib", width=600, height=400)
    dpg.setup_dearpygui()

    with dpg.window(label="Example Window", tag="primary"):
        dpg.add_text("Click the button to open a Matplotlib graph in a separate process")
        dpg.add_button(label="Open Graph", callback=open_graph)

    dpg.show_viewport()
    dpg.set_primary_window("primary", True)
    dpg.start_dearpygui()
    dpg.destroy_context()
```

