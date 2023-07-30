```
>>> from news.models import *

>>> p1=User.objects.create_user(username='Tom')
>>> p2=User.objects.create_user(username='Dan')
>>> Author.objects.create(author=p1)
<Author: Tom - 0>
>>> Author.objects.create(author=p2)
<Author: Dan - 0>

>>> Category.objects.create(name='Спорт')
<Category: Спорт>
>>> Category.objects.create(name='Политика')
<Category: Политика>
>>> Category.objects.create(name='Бизнес')
<Category: Бизнес>
>>> Category.objects.create(name='Происшествия')
<Category: Происшествия>

>>> author=Author.objects.get(id=1)
>>> author
<Author: Tom - 0>

>>> Post.objects.create(author=author, category_type='NW', title='sometitle', text='somebigtext')
<Post: Tom - 0 - 2023-07-30 22:54:59.899395+00:00 - sometitle>
>>> Post.objects.get(id=1).category.add(Category.objects.get(id=1))
>>> Post.objects.create(author=author, category_type='AR', title='sometitle_AR', text='somebigtext_AR')
<Post: Tom - 0 - 2023-07-30 22:55:22.861108+00:00 - sometitle_AR>
>>> Post.objects.get(id=2).category.add(Category.objects.get(id=3))
>>> Post.objects.create(author=author, category_type='AR', title='sometitle_AR_2', text='somebigtext_AR_2')
<Post: Tom - 0 - 2023-07-30 22:55:50.313914+00:00 - sometitle_AR_2>
>>> Post.objects.get(id=3).category.add(Category.objects.get(id=4))

>>> Comment.objects.create(post=Post.objects.get(id=1), user=Author.objects.get(id=1).author, text='somecomment')
<Comment: Tom - Tom - 0 - 2023-07-30 22:54:59.899395+00:00 - sometitle - 0>
>>> Comment.objects.create(post=Post.objects.get(id=2), user=Author.objects.get(id=1).author, text='somecomment')
<Comment: Tom - Tom - 0 - 2023-07-30 22:55:22.861108+00:00 - sometitle_AR - 0>
>>> Comment.objects.create(post=Post.objects.get(id=3), user=Author.objects.get(id=2).author, text='somecomment')
<Comment: Dan - Tom - 0 - 2023-07-30 22:55:50.313914+00:00 - sometitle_AR_2 - 0>
>>> Comment.objects.create(post=Post.objects.get(id=3), user=Author.objects.get(id=1).author, text='somecomment')
<Comment: Tom - Tom - 0 - 2023-07-30 22:55:50.313914+00:00 - sometitle_AR_2 - 0>

>>> Comment.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=2).like()
>>> Post.objects.get(id=3).dislike()
>>> Comment.objects.get(id=2).dislike()
>>> Comment.objects.get(id=3).like()
>>> Post.objects.get(id=1).rating
1

>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).rating
6

>>> Author.objects.get(id=1)
<Author: Tom - 0>
>>> a = Author.objects.get(id=1)
>>> a.update_rating()
>>> a.rating
18

>>> b = Author.objects.get(id=2)
>>> b.update_rating()
>>> a.rating
1

>>> best_author=Author.objects.all().order_by('-rating').values('author', 'rating')[0]
>>> best_author
{'author': 1, 'rating': 18}

>>> best_post=Post.objects.all().order_by('-rating').values('datetime', 'author', 'rating', 'title')[0]
>>> the_post=Post.objects.get(rating=best_post['rating'])
>>> the_post
<Post: Tom - 18 - 2023-07-30 22:54:59.899395+00:00 - sometitle>

>>> all_post_comments=the_post.comment_set.all()
>>> all_post_comments
<QuerySet [<Comment: 2023-07-30 22:57:33.634471+00:00 - Tom - 1 - somecomment...>]>
```