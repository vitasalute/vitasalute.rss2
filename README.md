import xml.etree.ElementTree as ET
from datetime import datetime
import os
import subprocess

# === GENERA RSS ===
rss = ET.Element("rss", version="2.0")
channel = ET.SubElement(rss, "channel")

ET.SubElement(channel, "title").text = "Vita Salute RSS Feed"
ET.SubElement(channel, "link").text = "https://vitasalute.github.io"
ET.SubElement(channel, "description").text = "Articoli aggiornati automaticamente"
ET.SubElement(channel, "language").text = "it-it"

# Aggiungi un articolo (modifica se vuoi automatizzare con altri contenuti)
item = ET.SubElement(channel, "item")
ET.SubElement(item, "title").text = "Articolo automatico"
ET.SubElement(item, "link").text = "https://vitasalute.altervista.org/articolo"
ET.SubElement(item, "description").text = "Contenuto generato il " + datetime.now().strftime('%d/%m/%Y')
ET.SubElement(item, "pubDate").text = datetime.now().strftime('%a, %d %b %Y %H:%M:%S +0000')

tree = ET.ElementTree(rss)
tree.write("feed.xml", encoding="utf-8", xml_declaration=True)

print("✅ feed.xml generato!")

# === GIT ADD, COMMIT, PUSH ===

subprocess.run(["git", "add", "feed.xml"])
subprocess.run(["git", "commit", "-m", f"Update feed.xml - {datetime.now().isoformat()}"])
subprocess.run(["git", "push", "origin", "main"])

print("🚀 feed.xml pubblicato su GitHub Pages!")
