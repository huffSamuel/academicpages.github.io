---
tags:
- flutter
title: Disable Form Button
posted: 2019-06-02 01:00:00 +0000
content: ''

---
    class MyFormDialog extends StatefulWidget {
    
      @override
      _MyFormDialog createState() => _MyFormDialog();
    }
    
    class _MyFormDialog extends State<MyFormDialog> {
      bool _buttonEnabled = false;
    
      final _formKey = GlobalKey<FormState>();
    
      @override
      Widget build(BuildContext context) {
        return SimpleDialog(
          title: Text('My Form Dialog'),
          contentPadding: EdgeInsets.fromLTRB(24.0, 8.0, 8.0, 8.0),
          children: <Widget>[
            Form(
              key: _formKey,
              autovalidate: true,
              onChanged: () {
                setState(() {
                  _buttonEnabled = _formKey.currentState.validate(); 
                 });
              },
              child: TextFormField(
                validator: (str) {
                  if(str.isEmpty) {
                  	return 'Required';
                  }
                },
              )
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: <Widget>[
              FlatButton(
                textColor: Theme.of(context).accentColor,
                child: Text('CANCEL'),
                onPressed: () {
                  Navigator.of(context).pop();
                },
              ),
              FlatButton(
                textColor: Theme.of(context).accentColor,
                child: Text('SAVE'),
                onPressed: _buttonEnabled ? () {
                  Navigator.of(context).pop();
                } : null,
              )
            ])
          ],
        );
      }
    }
    