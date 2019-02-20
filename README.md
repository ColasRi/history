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

## Trying to fix history with BFG

```bash
cd
git clone --mirror git@github.com:ColasRi/history.git
echo "Plan to take over the world" > to-remove.txt 
java -jar bfg-1.13.0.jar --replace-text to-remove.txt history.git
cd history.git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push
```

## Test

Hmm, I still see my secret in https://github.com/ColasRi/history/commit/6f2213642973aa55b1094ddf64a7a72c8154419b
I don't know if anyone could find it without the old commit ID, but it's still there.
