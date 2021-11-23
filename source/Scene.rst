#########
The Scene
#########

The hierarchical Representation
-------------------------------
If you want to build a virtal model of the world you can use trees. If you want to create a table with a glass staing on it you can model this like:
- The floor where is the table stands on is the root-node
- On the floor there is the table. So the table is a child of the floow-
- On the table there is the glass standing on it. So the glass can be modelled as the child of the table.

The advantage of this kinde of modelleing gets clearer ifd you want to shake the floor by an earthquake. 
For a planned animation you need to know which objects get affected by the earchquake. And here the hierachical tree 
from your scnene gan hekp you: you just need to traverse all children from the floor.

Nodes and more
--------------
So how can we describe the scene by using a tree. OSRE is using nodes. Each node can have one single parent node and a couple of children nodes:

```
class Node {
    Node mParent;
    ::CPPCore::TArray<Node*> mChildren;
};
```

Culling and Picking
-------------------
