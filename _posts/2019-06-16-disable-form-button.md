---
tags:
- flutter
title: 'Flutter: Disable Form Button'
collection: posts
permalink: /posts/disable_form_button
title: Disable Form Button
published: true
posted: 2019-06-02T01:00:00.000+00:00

---
Flutter is the cool new kid on the block when it comes to developing cross-platform mobile applications but since it is still so new it's hard to find good examples for everything. In this post I will show you how to create a form dialog that disables the save button until the form passes validation.

The form I am creating has the following requirements:
- Must be a dialog
- Single input field
- Input must be parsed as a double
- Save button should be disabled until the field is valid

Lets start by designing the dialog widget:

``` dart

class MyFormDialog extends StatefulWidget {
  @override
  _MyFormDialog createState() => _MyFormDialog();
}

class _MyFormDialogState extends State<MyFormDialog> {
  @override
  Widget build(BuildContext context) {
    return SimpleDialog();
  }
}

```

Anyone who has worked with Flutter already should be familiar with this boilerplate already. To those of you who are new to this go [here](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html) to read up on Flutter and it's widget system.

Now we need to add some actual input to our dialog where the user can enter the data that we need. Lets layout the dialog with our form, input field, and the save button.

``` dart
return SimpleDialog(
  children: <Widget> [
    Form(
      child: TextFormField(
        validator: (str) {
          if(str.isEmpty) {
            return 'Required';
          }
        },
      )
    ),
    FlatButton(
      textColor: Theme.of(context).accentColor,
      child: Text('SAVE'),
      onPressed: () {
        Navigator.of(context).pop();
      },
    )
  ]
);
```

Great! No styling so it's a little ugly, but it's got all the stuff we need but our save button is always valid. According to the material design specifications that's a no-go so we are going to need to disable that until the user provides us with good data. We are going to do that by assigning a `GlobalKey` to the form and updating the widget's state when our form changes.

Start by creating a global key as a final member of your widget's state and add a bool that will be our flag to indicate if the form is valid. We will be using these two members to control our button. At this point we are also going to add a controller for the text input field.
``` dart
bool _buttonEnabled = false;
final _formKey = GlobalKey<FormState>();
final _controller = TextEditingController();
```

Now let's update the form and the button to use these new members.
``` dart 
children: <Widget> [
  Form(
    key: _formKey,
    onChanged: () {
      setState(() { 
        _buttonEnabled = _formKey.currentState.validate();
      });
    },
    child: TextFormField(
      controller: _controller,
      validator: (str) {
        if(str.isEmpty) {
          return 'Required';
        }
        if(double.tryParse(str) == null) {
          return 'Invalid input';
        }
      }
    )
  ),
  FlatButton(
    child: Text('SAVE'),
    onPressed: _buttonEnabled ? () {
      Navigator.of(context).pop(double.parse(_controller.text));
    } : null;
  )
]
```

That's it! Now when the text in the form is updated the `onChanged` method will run and update the button's enabled state. If the form is not enabled the button's `onPressed` event is set to `null` and the button will be disabled.

Complete example:

``` dart
class MyFormDialog extends StatefulWidget {
  @override
  _MyFormDialog createState() => _MyFormDialog();
}

class _MyFormDialogState extends State<MyFormDialog> {

  bool _buttonEnabled = false;
  final _formKey = GlobalKey<FormState>();
  final _controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return SimpleDialog(
      children: <Widget> [
        Form(
          key: _formKey,
          onChanged: () {
            setState(() { 
              _buttonEnabled = _formKey.currentState.validate();
            });
          },
          child: TextFormField(
            controller: _controller,
            validator: (str) {
              if(str.isEmpty) {
                return 'Required';
              }
              if(double.tryParse(str) == null) {
                return 'Invalid input';
              }
            }
          )
        ),
        FlatButton(
          child: Text('SAVE'),
          onPressed: _buttonEnabled ? () {
            Navigator.of(context).pop(double.parse(_controller.text));
          } : null;
        )
      ]
    );
  }
}

```