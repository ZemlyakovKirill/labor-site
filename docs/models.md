
#БД "биржа труда"

##Модель "безработный"
###содержит ФИО, адрес проживания, контактный номер, паспортные данные,
###опыт работы, ссылка на организацию,
###в которой обучался безработный, имя-идентификатор, пароль, 
###адрес электронной почты

##Ограничения: 
###паспорт уникален, опыт работы больше или равен 0

```python
class Jobless(AbstractUser):
    address=models.CharField(max_length=500,null=True)
    lfm=models.CharField(max_length=200)
    tel=models.CharField(max_length=13)
    passport=models.CharField(max_length=15)
    workexp=models.IntegerField(null=True)
    organization=models.ForeignKey('EducationalOrganization',on_delete=models.CASCADE,null=True)
    class Meta:
        constraints = [
            models.UniqueConstraint(
                name="passport_unique",
                fields=["passport"],
            ),
            models.CheckConstraint(
                name="%(app_label)s_%(class)s_name_not_empty",
                check=models.Q(workexp__gte=0),
            ),
        ]

    def __str__(self):
        return '{}'.format(self.username)
```
##Модель "образовательная организация" 
###содержит наименование, тип, адрес

```python
class EducationalOrganization(models.Model):
    TYPE_EN=[('SPO','СПО'),
             ('SOO','СОО'),
             ('HS','ВО'),
             ('SO','СО')
    ]
    name=models.CharField(max_length=200)
    type=models.CharField(max_length=5,choices=TYPE_EN)
    address=models.CharField(max_length=500)
    def __str__(self):
        return '{}'.format(self.name)
```

##Модель "Пособие" 
###содержит ссылку на безработного, величина пособия, 
###дата начала предоставления, дата окончания предоставления

##Ограничения: 
###дата начала предоставления меньше или равна дате окончания предоставления, величина пособия больше или равна 0

```python
class Stipend(models.Model):
    jobless=models.ForeignKey('Jobless',on_delete=models.CASCADE)
    value=models.IntegerField()
    startprov=models.DateField()
    finprov=models.DateField()
    class Meta:
        constraints = [
            models.CheckConstraint(
                name="start_finprov",
                check=models.Q(startprov__lte=models.F("finprov")),
            ),
            models.CheckConstraint(
                name="value_check",
                check=models.Q(value__gte=0),
            ),
        ]
    def __str__(self):
        return '{} {}'.format(self.id,self.value)
```

##Модель "образовательная программа" 
###содержит наименование, дата начала обучения, 
###дата окончания обучения, стоимость, тип программы, описание

##Ограничения: 
###дата начала обучения меньше или равна дате окончания обучения, 
###стоимость больше или равна 0

```python
class EducationalProgram(models.Model):
    name=models.CharField(max_length=200)
    startdate = models.DateField()
    finishdate = models.DateField()
    cost= models.IntegerField()
    type=models.CharField(max_length=20)
    description=models.CharField(max_length=500,null=True)
    class Meta:
        constraints = [
            models.CheckConstraint(
                name="start_findate",
                check=models.Q(startdate__lte=models.F("finishdate")),
            ),
            models.CheckConstraint(
                name="cost_check",
                check=models.Q(cost__gte=0),
            ),
        ]
    def __str__(self):
        return '{}'.format(self.name)
```

##Модель "образовательная группа"
###содержит ссылку на образовательную программу,
###максимальное кол-во студентов, текущее кол-во студентов

##Ограничения:
### текущее кол-во студентов меньше или равно максимальному кол-ву студентов,
###текущее кол-во студентов больше или равно 0, 
###максимальное кол-во студентов больше или равно 0

```python
class EducationalGroup(models.Model):
    program=models.ForeignKey(EducationalProgram,on_delete=models.CASCADE)
    maxquanstud=models.IntegerField()
    studquan=models.IntegerField(default=0)
    pasg = models.ManyToManyField(Jobless, through='Passage')
    class Meta:
        constraints = [
            models.CheckConstraint(
                name="max_stud",
                check=models.Q(studquan__lte=models.F("maxquanstud")),
            ),
            models.CheckConstraint(
                name="stud_check",
                check=models.Q(studquan__gte=0),
            ),
            models.CheckConstraint(
                name="max_check",
                check=models.Q(maxquanstud__gte=0),
            ),
        ]

    def __str__(self):
        return '{} {}'.format(self.id,self.studquan)
```

##Модель "Прохождение" 
###содержит ссылку на безработного, ссылку на образовательную группу,
###статус принятия в группу, наличие документа об окончании

```python
class Passage(models.Model):
    jobless=models.ForeignKey(Jobless,on_delete=models.CASCADE)
    group=models.ForeignKey(EducationalGroup,on_delete=models.CASCADE)
    statofadopt=models.BooleanField(default=False)
    document=models.BooleanField(default=False)

    def __str__(self):
        return '{} {} {}'.format(self.id,self.jobless,self.group)
```