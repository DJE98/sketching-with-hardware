---
title: "Subtraktive Fertigung"
date: 2023-01-31T21:05:48+01:00
---
# Grundlagen 
In unserer letzten Sitzung haben wir uns mit subtraktiver Fertigung genauer beschäftigt. Generell spricht man von dieser Art der Fertigung, sobald etwas von einem Werkstoff abgetragen wird, um die finale Form zu erreichen. Sobald dieser Vorgang automatisch ausgeführt werden soll, wird eine numerische Computersteuerung engl. Computer Numerical Control (CNC) benötigt. Diesem System kann mittels G-Code mitgeteilt werden, wie es das Werkzeug ansteuern muss. So kann wiederkehrend und präzise die gewünschte Form des Objektes erhalten werden. Als Werkzeuge für die subtraktive Fertigung haben wir uns Laserschneider und Fräsen genauer angeschaut. 
# Laserschneider 
Ein Lasercutter eignet sich, um Objekte bis zu einer gewissen Dicke zu schneiden sowie diese zu gravieren. Damit die Werkstücke geringer bearbeitet werden können, kann die Fahrgeschwindigkeit erhöht sowie die Leistung des Lasers reguliert werden. An dieser Stelle sollte auf die Gefahr des Lasers für die Augen im Besonderen sowie für die Körper im Generellen hingewiesen werden. Deshalb muss der Laservorgang in einem geschlossenen Bereich durchgeführt werden. Auch sollte der Laser aufgrund seiner Leistung nur unter Aufsicht gefahren werden, da eine gewisse Brandgefahr besteht. Mithilfe des Lasercutters können auch nicht alle Werkstoffe gefahrenfrei verarbeitet werden, so können etwa bei der Verbrennung von Kunstleder giftige Gase entstehen. 
![Laserschnitt des Fallout Maskottchen](/posts/images/laserschnitt.jpg) 
Nun haben wir mit der Software Laserburn eine SVG-Datei in G-Code umgewandelt. Hierbei habe ich das Maskottchen aus dem Computerspiel Fallout gelasercuttet. Zur Unterstützung hat mir die Software unterschiedliche Voreinstellungen für Materialen zum Gravieren und Schneiden bereitgestellt. In meinem Fall habe ich die äußere Kante der Figur geschnitten und die inneren Linien graviert. Als Material habe ich schwarzes Acrylglas verwendet.i
# CNC Fräse 
Beim Fräsen müssen im Gegensatz zum Laserschneiden andere Besonderheiten bei der Verarbeitung berücksichtigt werden. Das Material wird mittels eines rotierenden Werkzeuges, eines Fräsers abgetragen. So muss die Dicke des Fräskopfs eingeplant werden. Auch muss der Fräskopf sowie die CNC Fräse als solches für die Verarbeitung des jeweiligen Werkstückes geeignet sein. Aus diesem Grund bestehen Fräsköpfe aus unterschiedlichen Werkstoffen wie Stahl, Hartmetall oder Diamanten abhängig vom Werkstoff. Außerdem sollte die Fräse den passenden Drehmoment erzeugen. Schließlich muss auch die Führung der Fräse die wirkenden Kräfte aushalten können. Beim Abtragen des Werkstoffes besteht Gefahr durch herumfliegende Splitterteile. Weshalb der Fräsvorgang nur in einem abgeschlossenen Bereich stattfinden sollte. Überdies sollte der Vorgang aufgrund von Brandgefahr überwacht werden.  Zur Erstellung von G-Code für die CNC Fräse haben wir uns die kommerzielle Software Carbide Create angeschaut. Diese unterstützt die Planung der Route des CNC Fräsers und weißt im Vorfeld auf bestehende Einschränkungen hin. 
# Nutzen für das Projekt 
![Skizze der Sanduhr](/posts/images/sanduhr_skizze.png) 
Momentan plane ich das Gehäuse der Sanduhr vollständig 3-D zu drucken, da es sich bei den Bauteilen um komplexere Formen handelt. Theoretisch könnte man die Grundflächen der Gehäuseteile aus Holz lasercutten oder fräsen und nur die Wandteile sowie die Halterungen für die Displays und den Raspberry Pi 3-D drucken und anschließend mit der Grundfläche verbinden. Dies könnte jedoch den künstlerischen Aspekt des Projektes stören. Auf jeden Fall wird das durchsichtige Acrylglas für das Innere der Sanduhr mittels Lasercutter in die passende Form geschnitten.