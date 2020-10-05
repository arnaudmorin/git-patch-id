# Show that patch-id could be different for commits that were cherry-picked

In branch b1, you have 3 commits:

```
git log --oneline b1
1218bb7 (b1) add line 5
1ec44b9 reverse line 2 and 3
5f43ba0 (HEAD -> master) init
```

In branch b2, you have 2 commits:

```
git log --oneline b2
9fe2478 (b2) add line 5
5f43ba0 (HEAD -> master) init
```


Commit 9fe2478 was cherry-picked from 1218bb7 in b1:

```
git show 9fe2478

commit 9fe24785fa18da2c0b83e66bac9fc6456cd75416 (b2)
Author: Arnaud Morin <arnaud.morin@ovhcloud.com>
Date:   Mon Oct 5 17:05:02 2020 +0200

    add line 5

    Signed-off-by: Arnaud Morin <arnaud.morin@ovhcloud.com>
    (cherry picked from commit 1218bb72dd5ba3b8fdd2ad0abf4bb5a91dd6afe7)

diff --git a/test b/test
index 276be75..c0cc4e7 100644
--- a/test
+++ b/test
@@ -7,3 +7,4 @@ line 2
 line 3

 line 4
+line 5

```

And 1218bb7 is :

```
git show 1218bb7

commit 1218bb72dd5ba3b8fdd2ad0abf4bb5a91dd6afe7 (origin/b1, b1)
Author: Arnaud Morin <arnaud.morin@ovhcloud.com>
Date:   Mon Oct 5 17:05:02 2020 +0200

    add line 5

    Signed-off-by: Arnaud Morin <arnaud.morin@ovhcloud.com>

diff --git a/test b/test
index 1653604..cdc7d2e 100644
--- a/test
+++ b/test
@@ -7,3 +7,4 @@ line 3
 line 2

 line 4
+line 5
```

But patch-id are different:

```
git show b1 | git patch-id
# patch-id commit-id
5e2b0b05899c659068af694907456657eb33b1f6 1218bb72dd5ba3b8fdd2ad0abf4bb5a91dd6afe7
```

```
git show b2 | git patch-id
# patch-id commit-id
f031536d15b0b26f3153b2a3f9ed64e54702da33 9fe24785fa18da2c0b83e66bac9fc6456cd75416
```

As a result, commit-id are different because tree is different.
Patch-id are also different, because diff | sha1 is different!

