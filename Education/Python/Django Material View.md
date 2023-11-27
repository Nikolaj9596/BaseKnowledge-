#django #python 
### Step 3. Create a materialized view in database

In PostgreSQL console, run following SQL command to generate a materialized view that only contains data whose pub_date is in or after year 2021.
```sql
create materialized view polls_question_materialized_view_2021 as  
select * from polls_question  
where pub_date>='2021-01-01'
```
We should see it under Materialized Views category as below.

![](https://miro.medium.com/max/417/1*2lr5G8B9SM5gagCL5LNhhw.png)

# Step 4. Build and associate model with materialized view

In **models.py** in **polls** app, create a model **Question2021** as below.
```python
class Question2021(models.Model):  
    question_text = models.CharField(max_length=200)  
    pub_date = models.DateField(null=True, default=None)    
    class Meta:  
        managed = False  
        db_table = 'polls_question_materialized_view_2021'
```

It’s important to declare db_table explicitly so that Django can make connection between model and materialized view. Please do not run migration. Because materialized view is already created manually through SQL command. Plus, we need to make sure **managed** attribute is False. It makes sure Django does not create table by mistake when we run migrations on **polls** app in the future.

# Step 5. Create lookup query on materialized model

Add lookup query by selecting all data on **Question2021** model and print number of returns in console. Codes should look like below.

```python
from django.shortcuts import render  
from polls.models import Question, Question2021

def index(request):  
    questions = Question.objects.all()  
	print('Origin table returns {} questions'.format(questions.count()))
	questions_2021 = Question2021.objects.all()  
    print('Materialized view returns {} questions.'.format(questions_2021.count()))return render(request, 'polls/index.html')

```

Launch project. Open up [http://127.0.0.1:8000/polls/](http://127.0.0.1:8000/polls/) in browser. We should see two prints as below in console. Materialized view returns less rows than origin table returns. It works as expected!

![](https://miro.medium.com/max/462/1*Cq1zKcIhg7TU8g7pHrFm8w.png)

# Conclusion

In this article, we’ve created materialized view in database and associate ORM model with it. It’s quite straightforward, is it? For architecture perspective, hope it gives you a better idea how to manage your data queries engine. Thanks for reading. Stay tuned.