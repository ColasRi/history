# BFG test

## The incriminating commit

```bash
git clone git@github.com:ColasRi/history.git
cd history
echo "A dummy README" > README
echo "Plan to take over the world" > secret
git add README secret 
git ci -m "First commit"
git push
```

## Fixing head

```bash
echo "removed manually in head" > secret 
git add secret 
git ci -m "fix head"
git push
```

## Fixing history with BFG

```bash
cd
git clone --mirror git@github.com:ColasRi/history.git
echo "Plan to take over the world" > to-remove.txt 
java -jar bfg-1.13.0.jar --replace-text to-remove.txt history.git
cd history.git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push
```

## Result

Mixed results:

- The git history is changed, the secret does not appear in the current history, eg: https://github.com/ColasRi/history/commit/310adae845547f6b07c251b5dfe269700b1778e0.
- But the old history is still on https://github.com/ColasRi/history/commit/6f2213642973aa55b1094ddf64a7a72c8154419b.
I don't know if anyone could find it without the old commit ID, but it's still there.
