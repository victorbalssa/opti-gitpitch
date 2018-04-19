# Bomberman extended

### Step 2: Memory optimization

---

## Before optimization

### commit d3d8539e478e11f3ec46cfd4be8db16b840eeda9

---?image=assets/image/before-opti.jpg

---

@title[C Code before optimization]


```c
	if (!(buff = my_str_to_wordtab(response[2], ';')))
		return ;
	for (int i = 0; i < MAP_SIZE; ++i) {
		if (!(buff2 = my_str_to_wordtab(buff[i], ':')))
			return ;
		for (int j = 0; j < MAP_SIZE; ++j) {
			g->map[i][j].data = buff2[j];
		}
	}
	free_wordtab(buff2, MAP_SIZE + 1);
	free_wordtab(buff, MAP_SIZE + 1);
	free_wordtab(response, 3);
```

@[1,2](get first table from server response)
@[3,9](update map from response data but without freeing old data)
@[10,12](freeing rest of table)

---

---

@title[C Code after optimization]


```c
	if (!(buff = my_str_to_wordtab(response[2], ';')))
		return ;
	for (int i = 0; i < MAP_SIZE; ++i) {
		if (!(buff2 = my_str_to_wordtab(buff[i], ':')))
			return ;
		for (int j = 0; j < MAP_SIZE; ++j) {
			tmp_swap = g->map[i][j].data;
			g->map[i][j].data = buff2[j];
			buff2[j] = tmp_swap;
		}
		free_wordtab(buff2, MAP_SIZE + 1);
	}
	free_wordtab(buff, MAP_SIZE + 1);
	free_wordtab(response, 3);
```

@[1,2](get first table from server response)
@[3,12](update map from response data and swapping pointer with tmp_sawp to free old data)
@[13,14](freeing rest of table)

---

## After optimization

### commit 59180f1db6fb6ac49509eefff0c870181bb813fe

---?image=assets/image/after-opti.jpp

---

## Question ? Live test ?
