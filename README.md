# Сервис YamDB
![yamdb](https://github.com/1kovalevskiy/yamdb/actions/workflows/main.yml/badge.svg)
![coverage](https://github.com/1kovalevskiy/yamdb/blob/master/coverage.svg)

## Учебный сервис "YamDB" 
Проект YamDB позволяет делиться отзывами на различные художественные (и не только) произведения.

Произведения делятся на категории: «Книги», «Фильмы», «Музыка». В каждой категории есть произведения.
Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.

Произведению может быть присвоен жанр из списка предустановленных (для музыки это рок, поп и т.д.; для фильмов это документальное, мультипликационный и т.д.; для книг это деловое, сказка и т.д.).

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти.
Из пользовательских оценок формируется усреднённая оценка произведения — рейтинг.

На одно произведение пользователь может оставить только один отзыв.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.


## Deploy
В корневой папке 
- Создать файл `.env` по примеру файла `.env.sample`
- Запустить `docker-compose up -d`

## Тестовый сервер
[Тестовый сервер (не работает)](http://yamdb.kovalevskiy.xyz)http://yamdb.kovalevskiy.xyz

## Технологии
- API на "Django + DRF"
- Тестирование на "Pytest"
- БД PostgreSQL
- Proxy Nginx
- Контейнеризация с помощью Docker


## Техническая информация
### Пользовательские роли
- Аноним — может просматривать описания произведений, читать отзывы и комментарии.
- Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

### Самостоятельная регистрация новых пользователей
- Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
- Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
- Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).

### Ресурсы API YaMDb
- Ресурс auth: аутентификация.
- Ресурс users: пользователи.
- Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
- Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
- Ресурс genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
- Ресурс reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
- Ресурс comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.
