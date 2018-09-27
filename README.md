# Git bisect and revert

## โจทย์คือหาจุดที่ไฟล์ c โดนลบออกจาก repository แล้วยกเลิกการเปลี่ยนแปลงนั้น(เอาไฟล์ c กลับมา)

## git bisect

* clone project

```
git clone xxxxxx
cd xxxx
```

* ดูไฟล์ จะพบว่าจะเรียงลำดับ a, b, d, e, f

```
ls
a b d e f
```

* เริ่มต้นใช้ git bisect

```
git bisect start
```

* บอก git bisect ว่าจุดที่รันคำสั่งนี้ไม่มี ไฟล์ที่ชื่อว่า c

```
git bisect bad
```

* หาจุดที่รู้ว่ามีไฟล์ที่ชื่อว่า c อยู่ใน repository ในที่นี้คือ [9a6a9c5, c90df04, 6573e8f]

```
git log --oneline
```

```
c1d8c56 (HEAD -> master) Create f file.
108b663 Create e file.
430ee00 Remove c
0fa9e88 Create d file
6573e8f Create c file
c90df04 Create b file.
9a6a9c5 Init project with a file.
```

* บอก git bisect ว่าจุดที่รู้ว่ามีไฟล์ที่ชื่อว่า c อยู่ใน repository หรือยังทำงานถูกต้อง

```
git bisect good 6573e8f
```

```
Bisecting: 1 revision left to test after this (roughly 1 step)
[430ee0063fb9d02955052956f4d0bb7cf78811b6] Remove c

// git bisect จะขยับตัวเองไปใน commit ถัดไป 
//เพื่อให้เราตรวจดูว่ามันทำงานถูกหรือผิด ในที่นี้คือ หาไฟล์ c
```

* ตรวจแล้วบอก git bisect ว่า good / bad

```
ls
a b d // ไฟล์ c ไม่มี

git bisect bad
```

```
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[0fa9e88e9c6b686cf5907df1f0849a61b2baa104] Create d file
```

* ตรวจแล้วบอก git bisect ว่า good / bad
```
ls
a b c d

git bisect good
```

```
430ee0063fb9d02955052956f4d0bb7cf78811b6 is the first bad commit
commit 430ee0063fb9d02955052956f4d0bb7cf78811b6
```

* step การทำงานจะวนแบบนี้ไป จนกว่าเราจะเจอคำว่า

```
[commit ID] is the first bad commit
```

* ตอนนี้เราเจอแล้วว่า commit ID 430ee0063fb9d02955052956f4d0bb7cf78811b6 คือจุดแรกที่ไฟล์ c หายไป

* ออกจาก git bisect

```
git bisect reset
```

## git revert

* ยกเลิกการเปลี่ยนแปลงที่เกิดขึ้น ในที่นี้คือยกเลิกการ ลบไฟล์ c

```
git revert 430ee0063fb9d02955052956f4d0bb7cf78811b6
```

* ตรวจดูว่า ไฟล์ c กลับมารึยัง

```
ls
a b c d e f
```

ref: [https://git-scm.com/docs/git-bisect](https://git-scm.com/docs/git-bisect)