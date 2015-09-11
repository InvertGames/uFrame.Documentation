# Moving nodes between graphs

As part of development it is common that you will end up putting nodes in the wrong place or deciding to refactor your structure later, such as moving generic or re-useable nodes to its own sub system graph while keeping game specific information in a tightly coupled game MVVM graph.

uFrame supports moving of nodes between graphs and its quite simple, to begin with go to the location where you want to move a node to, then right click and in the menu go to **Show** and then select the node you want to move. Once you have done this you will notice that there is a transparent instance of the node in your destination location. From here right click the node and select **Pull From**, this will move the node from its current location in whatever graph it is in, to your new location.

Remember that this will only move one node at a time so any dependencies of this newly re-located node will still link back to the nodes in the old graph, so you may need to move them over too.
