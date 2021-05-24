
#Адреса

##Стартовая страница
```python
path('home/', views.getAction),
```
##Регистрация безработного
```python
    path('SignUp/', views.registration),
```
##Подключение адресов авторизации, встроенных в django
```python
    path('', include('django.contrib.auth.urls')),
```
##Выход из аккаунта
```python
    path('LogOut/',LogoutView.as_view(),name="logout"),
```
##Детализация безработного
```python
    path('Jobless/Detail/',views.detail_jobless),
```
##Список образовательных организаций
```python
    path('Organization/List',views.OrganizationList.as_view()),
```
##Обновление образовательной организации
```python
    path('Organization/Update/<int:pk>',views.OrganizationUpdate.as_view()),
```
##Удаление образовательной организации
```python
    path('Organization/Delete/<int:pk>',views.OrganizationDelete.as_view()),
```
##Добавление образовательной организации
```python
    path('Organization/Create',views.OrganizationCreate.as_view()),
```
##Список пособий
```python
    path('Stipend/List',views.StipendList.as_view()),
```
##Обновление пособия
```python
    path('Stipend/Update/<int:pk>',views.StipendUpdate.as_view()),
```
##Удаление пособия
```python
    path('Stipend/Delete/<int:pk>',views.StipendDelete.as_view()),
```
##Добавление пособия
```python
    path('Stipend/Create',views.StipendCreate.as_view()),
```
##Список образовательных программ
```python
    path('Program/List',views.ProgramList.as_view()),
```
##Обновление образовательной программы
```python
    path('Program/Update/<int:pk>',views.ProgramUpdate.as_view()),
```
##Удаление образовательной программы
```python
    path('Program/Delete/<int:pk>',views.ProgramDelete.as_view()),
```
##Добавление образовательной программы
```python
    path('Program/Create',views.ProgramCreate.as_view()),
```
##Список образовательных групп
```python
    path('Group/List',views.GroupList.as_view()),
```
##Обновление образовательной группы
```python
    path('Group/Update/<int:pk>',views.GroupUpdate.as_view()),
```
##Удаление образовательной группы
```python
    path('Group/Delete/<int:pk>',views.GroupDelete.as_view()),
```
##Добавление образовательной группы
```python
    path('Group/Create',views.GroupCreate.as_view()),
```
##Список досупных пользователю групп
```python
    path('AvailGroups/List/',views.avial_groups),
```
##Присоединение пользователя к образовательной группе
```python
    path('JoinGroup/<int:pk>',views.join_group),
```
##Список прохождений
```python
    path('Passage/List',views.PassageList.as_view()),
```
##Создание прохождения
```python
    path('Passage/Create',views.PassageCreate.as_view()),
```
##Удаление прохождения
```python
    path('Passage/Delete/<int:pk>',views.PassageDelete.as_view()),
```
##Обновление прохождения
```python
    path('Passage/Update/<int:pk>', views.PassageUpdate.as_view()),
```
##Посмотреть участников группы
```python
    path('ViewMember/<int:g_id>',views.view_member),
```
##Список групп пользователя
```python
    path('MyGroups/List', views.my_groups_list),
```
##Выход пользователя из группы
```python
    path('LeaveGroup/<int:pk>',views.leave_group),
```
##Список безработных
```python
    path('Jobless/List',views.JoblessList.as_view()),
```
##Обновление безработного
```python
    path('Jobless/Update/<int:pk>',views.JoblessUpdate.as_view(success_url='/Jobless/Detail')),
``` 
##Удаление пользователя
```python
    path('DeleteUser',views.delete_user)
```