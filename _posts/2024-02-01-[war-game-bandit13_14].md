---
title: "[wargame] overthewire bandit 13 -> 14"
date: 2024-02-16 00:25:15 +09:00
categories: [war game, Linux]
tags: [bandit, overtherwire]
pin: true
---

## Bandit Level 13 -> Level 14

**user_id** : bandit13<br/>
**password** : wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

### 목표

![bandit12_14](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/6859c2c8-b01d-4c9c-9d19-be59b98dfd64)

비밀번호는 `/etc/bandit_pass/bandit14`에 있고 이 파일은 `bandit14`유저만 열어볼 수 있다. 이번 레벨에서는 다음 레벨로 가는 비밀번호는 찾을 수 없지만, 다음 로그인 할 때 필요한 `SSH key`는 가지고 있다. **Note : localhost**는 당신이 현재 작업하고 있는 `PC` 입니다.

### 해결법

이게 무슨 말인고 하니, '우리는 `ssh` 즉 네트워크 상의 다른 컴퓨터에 로그인, 혹은 원격 제어를 할 수 있게 해주는 프로토콜의 비밀번호를 이미 알고있다.' 라고 합니다. 일단 그럼 `SSH`가 무엇인지 부터 한번 이야기를 해볼까한다.
