Secturity Vonerability
----------------------

Routing
=======

vonerable
```
@order = orders.find(params[:id])
```

not so
```
@order = current_user.orders.find(params[:id])
```

Mass Assignment
===============
Be selective with attr_acsesable

Cross Site Scriping
===================
A user embeds code in the HTML

`gem breakman` security auditting gem

