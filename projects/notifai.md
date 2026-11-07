# How NotifAI opens a folder tree

In NotifAI, someone can keep a folder called **Rehearsal** inside **Music**. In MongoDB each folder has its own document. In Flutter the same two folders need to appear as a parent and child.

When the client requests `GET /api/directories/tree/`, the Django handler starts with folders whose `parentId` is null. For each one it looks up the documents whose `parentId` matches that folder’s `id`. It repeats that lookup down the tree.

With two example folders, the client reads these fields from the response.

```json
[
  {
    "id": "music",
    "name": "Music",
    "parentId": null,
    "children": [
      { "id": "rehearsal", "name": "Rehearsal", "parentId": "music" }
    ]
  }
]
```

The Flutter client walks the nested response and builds its folder objects. It treats a missing `children` field as an empty list, so **Rehearsal** appears as a leaf.

One detail I’d revisit for larger libraries is the number of database reads. This example takes one lookup for the roots, one for the children of **Music**, and one to discover that **Rehearsal** has no children. For a larger tree, I’d compare that with fetching the folders together and grouping them by parent in memory.

[Back to selected projects](../README.md#selected-projects)
