---
title: "Yearn объясняет, что такое хранилища(vaults) и как работают стратегии?"
image:
  src: ./image3.jpg
  width: 1024
  height: 597
date: '2022-04-02'
author: Marco_Worms
translator: Ivan Manvelov
---

![](./image1_ru.jpg?w=900&h=478)\
*Простой пример хранилища Yearn Ethereum*

[Yearn Finance](http://yearn.finance/) это набор продуктов в области децентрализованных финансов (DeFi), который обеспечивает генерацию доходности и агрегирование доходности для вложенных средств и многое другое в блокчейне. Протокол поддерживается различными независимыми разработчиками и управляется держателями токенов YFI.

Текущим основным продуктом Yearn, ориентированным на пользователя, является **yVault**, который обеспечивает автоматическую доходность для множества различных криптоактивов, каждый из которых управляется одной или несколькими **стратегиями**. Дизайн yVault является открытым, а значит другие протоколы могут внедряться поверх Yearn, как в случае, [Abracadabra + Yearn](https://twitter.com/MarcoWorms/status/1483223651684081670).

# Хранилища Yearn(yVaults)

Короткое название для Yearn Vault - yVault. В текущей версии Yearn (v2) характеристики yVault следующие:

- **Токен, который вы вносите в yVault - это токен, который вы получаете в качестве дохода**, и он всегда автоматически добавляется в yVault.
- **В yVault может быть несколько активных стратегий одновременно.** YVault может изменить распределение капитала в своих стратегиях, когда сочтет это необходимым.
- В отличие от многих других агрегаторов доходности, **с пользователя не взимается комиссия за ввод/вывод средств**.
- **Токены yVaults поддерживают стандарт ERC20**, это означает, что их можно легко перемещать между кошельками и рынками и использовать любое приложение, поддерживающее этот стандарт (например, децентрализованные биржи).

## Стратегии и стратеги

**Стратеги** - это люди, разрабатывающие одну или несколько основных **стратегий** для yVaults.

[Любой может создать стратегию](https://docs.yearn.finance/developers/v2/getting-started), но для того, чтобы добавить ее в yVault, стратег должен пройти через [процесс проверки стратегии](https://docs.yearn.finance/developers/v2/getting-started#overview-of-our-vetting-process) который включает проверку концепции, проверку кода, проверку безопасности и тестирование основной сети.

![](./image2.jpg?w=4000&h=588)\
*Процесс проверки стратегии*

За свои усилия стратеги вознаграждаются частью комиссий за эффективность стратегии.
- До 10% от сгенерированной доходности по конкретной стратегии (комиссии за эффективность) идет стратегу
- 10% от сгенерированных сборов по всем стратегиям (комиссии за эффективность) поступает в казну Yearn DAO.
- В течение года 2% от общих активов хранилища берутся в качестве сборов, которые идут Yearn на оплату таких расходов, как газ, гранты для разработчиков и другие услуги.

Теперь, когда мы знаем, что такое **yVaults(хранилища yearn)** и **Стратегии**, давайте углубимся в детали их работы.

# Погружаемся глубже в детали работы хранилищ и стратегий

### Разберем на примере одну стратегию

![](./image3_ru.jpg?w=1024&h=597)\
*Спасибо за эту схему FinematicsСпасибо за эту схему Finematics*

Изображение выше представляет собой обзор стратегии хранилища Ethereum в версии 1(v1) yVault. yVaults сейчас находятся в версии 2(v2) и могут обрабатывать несколько стратегий одновременно, но в этом примере мы сосредоточимся на одной стратегии. [Существует целый пост и видео Finematics](https://finematics.com/yearn-vaults-eth-vault-explained/) о том, как это работает, на случай, если вы захотите погрузиться глубже!

В этом примере мы видим, как стратегия может использовать другие хранилища! В стратегии Ethereum v1 yVault:

- Когда пользователь вносит ETH, он затем предоставляется MakerDAO в качестве залога.
- Залог используется для заимствования DAI
- Заимствованный DAI помещается в хранилище DAI yVault.

То есть получается, что мы используем ETH для заимствования DAI и получения дохода с помощью стратегии DAI yVault.

### Как/когда Yearn перемещает средства в хранилище и взимает комиссию?

Одна из ключевых функций стратегии называется «урожай»(harvest). После запуска этой функции начинается процесс перебалансировки, который приносит прибыль. Полученная прибыль сразу реинвестируется обратно в стратегию.

### Как Yearn гарантирует, что стратегия всегда генерирует токены, а не теряет их?

Стратеги используют ряд инструментов для мониторинга данных в сети, чтобы удостовериться в работоспособности стратегии. Одним из таких инструментов является [Yearn Watch](https://yearn.watch/), который представляет собой приятный пользовательский интерфейс, где показаны ключевые метрики в режиме онлайн.
Важно мониторить стратегии после запуска и следить за их эффективностью, но не менее важной является проверка до запуска, когда еще не используются реальные средства наших пользователей.

У команды Yearn, которая занимается стратегиями, также есть «Система оценки стратегии», которая оценивает уровень риска для используемых базовых стратегий, и мы надеемся, что в будущем мы сможем лучше представить её нашим пользователям в нашей документации и приложениях!

### Стратегии имеют ограничения, которые мы установили исходя из предыдущего опыта работы с хранилищами.

- Средства хранилища должны « только расти», а не уменьшатся
- Избегаем непостоянных потерь (например, не предоставлять ликвидность YFI/ETH в пуле ликвидности)
- Пользователи должны иметь возможность снимать средства в любое время (поэтому стратегия не может блокировать все средства в хранилище, а только небольшую их часть).
- Используем только протоколы с проверенной репутацией, понятной логикой и с неизменными контрактами.

### Keep3rs и yVaults

У Yearn и [Keep3r](https://docs.keep3r.network/) действительно сильная синергия: Keep3r используется в хранилищах(yVaults) для автоматизации общих задач!

Хранителям может быть полезна функция сбора урожая(harvest), когда это имеет смысл для yVault, например, если происходят следующие события:

- Стратегия заработала X (сумма) прибыли
- Прошло Y (количество) времени с момента последнего сбора урожая
- Потери невозможны если срабатывает функция сбора урожая(harvest)

И таких случаев много. Другим примером может быть тот случай, когда Хранитель начинает перебалансировку распределения активов хранилища, чтобы избежать ликвидации во время выполнения стратегии.

![](./image4.jpg?w=562&h=651)

### Построение стратегий

- **yVaults** написаны на языке программирования [Vyper](https://vyper.readthedocs.io/en/stable/)
- **Стратегии** написаны на языке программирования [Solidity](https://docs.soliditylang.org/en/v0.8.11/)

> Вам не нужно быть продвинутым разработчиком или финансовым аналитиком, чтобы стать стратегом!

В то время как обслуживание хранилищ(yVaults) является более сложным вопросом в части разработки, стратегии были разработаны для того, чтобы каждый мог их написать. Требования для написания хорошей стратегии:

- Знание экосистемы того блокчейна, для которого вы собираетесь разрабатывать стратегию(если это сеть Ethereum, то вам нужно разбираться в нюансах этой сети). Это можно сделать путем углубленного изучения токеномики и документации для всех токенов, используемых в самой стратегии.
- Знание языка программирования Solidity[на уровне аналогичном прохождению уровня 4 в CryptoZombies](https://cryptozombies.io/)
- Знание сервисов [git](https://git-scm.com/), [eth-brownie](https://eth-brownie.readthedocs.io/en/stable/), и [ganache](https://trufflesuite.com/ganache/).
- После понимания основ вышеперечисленных инструментов вы можете [копировать наш шаблон стратегии!](https://github.com/yearn/brownie-strategy-mix) Функции, которые вы должны начать изменять в этом шаблоне, чтобы создать свою собственную первую стратегию, - это prepareReturn, AdjustPosition и LiquidatePositon.

И последнее: после того, как Yearn одобрила стратегию и запустила ее в производство, вы должны помочь мониторить её!

### Узнать больше

Если вам нужно больше информации о хранилищах и стратегиях, ознакомьтесь с этими ресурсами! Все они помогли мне понять концепции, изложенные в этой статье, а также Yearn contributoooooors - самые добрые люди, которые всегда помогают мне найти лучший ресурс для получения качественной информации по каждой теме.

- [Описания yVaults](https://vaults.yearn.finance/)
- [Документация yVaults](https://docs.yearn.finance/getting-started/products/yvaults/overview)
- [Станьте могучим стратегом](https://www.youtube.com/watch?v=NVR3teJw0Y0)
- [Видео/статья от Finematics про yVaults](https://finematics.com/yearn-vaults-eth-vault-explained/)
- [Дополнительные ресурсы от Yearn](https://docs.yearn.finance/developers/v2/additional-resources)

### Примите синюю таблетку!

Если вам понравились наши рассказы про Vaults и Strategies:

- Будьте в курсе наших последних новостей в Твиттере Yearn Finance
- Прочтите нашу книгу «[Синяя таблетка](https://thebluepill.eth.link/)», в которой рассказывается о видении и истории Yearn
- И узнайте о [присоединении к команде Yearn](https://yearnfinance.notion.site/Join-Us-3e9c95b9bd7846a18c0f1cbe6ab05eda)!

*Продьюсер: [Worms](https://twitter.com/MarcoWorms), Рецензент: [Wavey](https://twitter.com/wavey0x)*

*Спасибо Farrah и Weaver за помощь в адаптации в Yearn и знакомство с замечательными людьми и ресурсами, которые позволили мне написать здесь эту первую статью!*

Сделано в [yearn.finance](https://yearn.finance)