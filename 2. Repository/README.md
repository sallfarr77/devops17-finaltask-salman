# Repository

Disini kita akan membuat dua repository di github dengan menggunakan ubuntu.

Pertama-tama kita akan menginstall github-cli dengan comand :
```
sudo snap install gh
```
setelah itu kita login dengan cara

```
gh auth login
```

![image](/2.%20Repository/img/1.png)

Untuk membuat repository private kita bisa menggunakan

```
gh repo create [<name>] ---private
```

Selanjutnya untuk langkah-langkah setup repositorynya 

```
git init
```

```
git add .
```

```
git commit -m "first-commit"
```

```
git branch -M <your branch name>
```

```
git remote add origin git@github.com:username/reponame
```

```
git remote set-url origin git@github.com:username/reponame
```

Dan yang terakhir kita upload repository lokal kita ke github-web dengan

```
git push origin <your branch name>
```

Karna disini saya akan membuat dua branch berbeda dengan nama staging dan production maka kita bisa membuat branchnya dengan cara :

```
git branch staging
```

```
git branch production
```

![image](/2.%20Repository/img/2.png)

![image](/2.%20Repository/img/3.png)





