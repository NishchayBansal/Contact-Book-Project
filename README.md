# Contact-Book-Project

```python
import tkinter as tk
from tkinter import messagebox
import json, os

FILE = "contacts.json"
contacts = json.load(open(FILE)) if os.path.exists(FILE) else {}

def save(): json.dump(contacts, open(FILE, "w"), indent=2)
def refresh(): 
    listbox.delete(0, tk.END)
    for k,v in contacts.items(): listbox.insert(tk.END, f"{k} - {v['phone']}")

def add():
    n, p, e, a = name.get(), phone.get(), email.get(), addr.get()
    if not n or not p: return messagebox.showerror("Error", "Name & Phone required")
    contacts[n] = {"phone": p, "email": e, "address": a}
    save(); refresh(); clear()

def load(event):
    try:
        sel = listbox.get(listbox.curselection())
        n = sel.split(" - ")[0]
        name.delete(0,tk.END); name.insert(0, n)
        phone.delete(0,tk.END); phone.insert(0, contacts[n]['phone'])
        email.delete(0,tk.END); email.insert(0, contacts[n]['email'])
        addr.delete(0,tk.END); addr.insert(0, contacts[n]['address'])
    except: pass

def delete():
    n = name.get()
    if n in contacts: del contacts[n]; save(); refresh(); clear()

def search():
    key = s.get().lower()
    listbox.delete(0, tk.END)
    for k,v in contacts.items():
        if key in k.lower() or key in v["phone"]: listbox.insert(tk.END, f"{k} - {v['phone']}")

def clear(): [e.delete(0, tk.END) for e in (name, phone, email, addr)]

root = tk.Tk(); root.title("📒 Contact Book"); root.geometry("420x530")
labels = ["Name", "Phone", "Email", "Address"]
for i, l in enumerate(labels): tk.Label(root, text=l).grid(row=i, column=0, sticky="w")
name = tk.Entry(root, width=30); name.grid(row=0, column=1)
phone = tk.Entry(root, width=30); phone.grid(row=1, column=1)
email = tk.Entry(root, width=30); email.grid(row=2, column=1)
addr = tk.Entry(root, width=30); addr.grid(row=3, column=1)

tk.Button(root, text="Add / Update", command=add).grid(row=4, column=1, pady=5)
tk.Button(root, text="Delete", command=delete).grid(row=5, column=1, pady=2)
tk.Button(root, text="Clear", command=clear).grid(row=6, column=1, pady=2)

tk.Label(root, text="Search:").grid(row=7, column=0)
s = tk.Entry(root, width=20); s.grid(row=7, column=1, sticky="w")
tk.Button(root, text="Search", command=search).grid(row=7, column=1, sticky="e")

tk.Label(root, text="Saved Contacts:").grid(row=8, column=0, columnspan=2)
listbox = tk.Listbox(root, width=45, height=12); listbox.grid(row=9, column=0, columnspan=2)
listbox.bind("<<ListboxSelect>>", load)

refresh()
root.mainloop()
```

<img width="1467" height="779" alt="image" src="https://github.com/user-attachments/assets/8b60b9ed-9edb-4893-b3bf-ad14b2992ff1" />

