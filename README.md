# anoma-node-instalation-guide

# [English](#english-version)
# [Russian](#русская-версия)

# English version

Link to the official documentation where you could find more details: https://docs.anoma.network/v0.2.0/index.html

The purpose of this documentation to guid you quickly through a Anoma instalation and test happy path.

Here are the list of steps that were tested on `Ubuntu 20.04` version to run Anoma node

## Step 1 - install all needed packages

`sudo apt update && sudo apt upgrade -y`

`sudo apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux`

## Step 2 - install Rust

`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

`source $HOME/.cargo/env`

## Step 3 - clone anoma repository

`cd $HOME`

`git clone https://github.com/anoma/anoma.git`

## Step 4 - checkout to the needed Anoma version

You should enter `anoma` folder using following command:

`cd $HOME/anoma`

When you are in the `anoma` folder you should execute following command:

`git checkout v0.2.0`

## Step 5 - copy `wasm` folder to the `$HOME` direcory 

`cp -a $HOME/anoma/wasm $HOME/wasm`

## Step 6 - install Anoma from the source

It is better to execute instalation in the seperate session, because it is take time. You could use `tmux` to create seperate session using following command: 

`tmux new -s session1`

When you are in the new session please enter `anoma` folder: 

`cd $HOME/anoma`

And execute the install process: 

`make install`

Wait for the instalation to complete.

## Step 7 - configure your node to join the Feigenbaum public testnet

`cd $HOME`

`anoma client utils join-network --chain-id=anoma-feigenbaum-0.ebb9e9f9013`

## Step 8 - generate new key in the wallet

`anoma wallet key gen --alias my-key`

## Step 9 - start a local Anoma ledger node

It is better to start it in the separate session or using systemd. In this example I will use `tmux`.

`tmux new -s anoma`

`anoma ledger`

Wait for the node to be synced

## Step 10 - Initialize an account

`anoma client init-account --alias my-new-acc --public-key my-key --source my-key`

After successful execution you should see something like this:

`The transaction initialized 1 new account
Added alias my-new-acc for address atest1kjdslahfaksjdhfajdhfkjadshfjkdfakjdhfakjlakdhfjhakdjhadkjfhaksdf`

## Step 11 - Interect with PoS 

### Step 11.1 - get some tokens from the faucet

`anoma client transfer --source faucet --target my-new-acc --signer my-new-acc --token XAN --amount 1000`

### Step 11.2 - check the balance

`anoma client balance --token XAN --owner my-new-acc`

You should see `XAN: 1000`

### Step 11.3 - register a validator

`anoma client init-validator --alias my-validator --source my-new-acc`

`my-validator` is a name of your validator

During execution you will need to enter encryption password, please save it to the save place.

After successful execution you should see following information:

```
Added alias my-validator for address atest1vakldhfalksdjhflksajdhfkjashdfkljhsad.
Added alias my-validator-rewards for address atest1v4eaksdfkahdlkfjhasldkjhflakjs.

The validator's addresses and keys were stored in the wallet:
  Validator address "my-validator"
  Staking reward address "my-validator-rewards"
  Validator account key "my-validator-key"
  Consensus key "my-validator-consensus-key"
  Staking reward key "my-validator-rewards-key"
The ledger node has been setup to use this validator's address and consensus key.
```

### Step 11.4 - restart you node

You should restart previously running `anoma ledger` process

### Step 11.5 - delegate tokens to the validator node

`anoma client bond --source my-new-acc --validator my-validator --amount 900.00`

You can query your delegations:

`anoma client bonds --owner my-new-acc`

# Русская версия

Ссылка к официальной документации: https://docs.anoma.network/v0.2.0/index.html

Список действий которые необходимо сделать на `Ubuntu 20.04` для того что бы запустить Anoma ноду

## Step 1 - инсталлируем необходимые пакеты

`sudo apt update && sudo apt upgrade -y`

`sudo apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux`

## Step 2 - инсталлируем  Rust язык

`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

`source $HOME/.cargo/env`

## Step 3 - клонируем репозиторий

`cd $HOME`

`git clone https://github.com/anoma/anoma.git`

## Step 4 - переключаемся в репозитории на нужную версию Anoma

Заходим в папку `anoma`:

`cd $HOME/anoma`

Когда вы находитесь в папке `anoma` вам необходимо выполнить следующую команду:

`git checkout v0.2.0`

## Step 5 - копируем `wasm` папку в `$HOME` директорию

`cp -a $HOME/anoma/wasm $HOME/wasm`

## Step 6 - инсталируем Anoma

Инсталяцию лучше проводить в отдельной сессие. Вы можете использовать `tmux` для создания отдельной сессии: 

`tmux new -s session1`

После создания сессии заходим в папку `anoma`: 

`cd $HOME/anoma`

Выполняем команду: 

`make install`

Ждем окончания инсталляции.

## Step 7 - конфигурируем ноду для тестнетнета Feigenbaum

`cd $HOME`

`anoma client utils join-network --chain-id=anoma-feigenbaum-0.ebb9e9f9013`

## Step 8 - генерируем новый ключ в кошельке

`anoma wallet key gen --alias my-key`

## Step 9 - стартуем Anoma ноду

В этом примере мы будем использовать `tmux` для старта ноды в отдельной сессие.

`tmux new -s anoma`

`anoma ledger`

Ждем синхронизации ноды

## Step 10 - Инициализируем счет

`anoma client init-account --alias my-new-acc --public-key my-key --source my-key`

После удачного выполнения вы должны видеть такое сообщение:

`The transaction initialized 1 new account
Added alias my-new-acc for address atest1kjdslahfaksjdhfajdhfkjadshfjkdfakjdhfakjlakdhfjhakdjhadkjfhaksdf`

## Step 11 - создание валидатора 

### Step 11.1 - запрашиваем токены

`anoma client transfer --source faucet --target my-new-acc --signer my-new-acc --token XAN --amount 1000`

### Step 11.2 - проверяем баланс

`anoma client balance --token XAN --owner my-new-acc`

Вы должны видеть `XAN: 1000`

### Step 11.3 - регестрируем валидатора

`anoma client init-validator --alias my-validator --source my-new-acc`

`my-validator` - это имя вашего валидатора

При выполнении нужно будет вводить пароль. Сохраните его.

После удачного выполнения вы будете видеть такое сообщение:

```
Added alias my-validator for address atest1vakldhfalksdjhflksajdhfkjashdfkljhsad.
Added alias my-validator-rewards for address atest1v4eaksdfkahdlkfjhasldkjhflakjs.

The validator's addresses and keys were stored in the wallet:
  Validator address "my-validator"
  Staking reward address "my-validator-rewards"
  Validator account key "my-validator-key"
  Consensus key "my-validator-consensus-key"
  Staking reward key "my-validator-rewards-key"
The ledger node has been setup to use this validator's address and consensus key.
```

### Step 11.4 - рестартуем ноду

Необходимо выключить и включить `anoma ledger` процесс

### Step 11.5 - делегируем токены созданному валидатору

`anoma client bond --source my-new-acc --validator my-validator --amount 900.00`

Проверяем что делегация произошла:

`anoma client bonds --owner my-new-acc`

