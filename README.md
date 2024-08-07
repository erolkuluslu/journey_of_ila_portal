
# Crafting Exceptional Mobile UI with Flutter: The Journey of ILA Portal

The ILA Portal mobile app is designed to offer users access to a variety of features, including the ability to apply calls and explore engaging content such as blogs, projects, and news. With a user-friendly interface and powerful functionality, the app promises to be a tool for those seeking to stay connected with the latest developments and opportunities within the ILA community.

![image](https://github.com/user-attachments/assets/c57e7077-66e7-408b-b2d8-318e927c5198)

## Why Flutter?

We chose to use Flutter to build the ILA Portal because it enables the creation of customized applications and offers numerous advantages that enhance performance, UI design, development efficiency, and cross-platform compatibility.

- **Performance:** Flutter’s engine compiles to native ARM code, ensuring high performance across devices. Leveraging the Skia graphics library, Flutter enables fast rendering and smooth animations, delivering a responsive and seamless user experience.

- **UI and Widget Capabilities:** Flutter offers a rich set of pre-designed widgets that are not only highly customizable but also composable, allowing developers to create complex UIs effortlessly. The pixel-perfect widgets ensure a consistent look and feel across both iOS and Android platforms, making your app visually appealing and professional.

- **Tree Structure:** Flutter’s hierarchical tree structure for building UIs makes it easy to manage and organize components. This approach enhances maintainability and scalability. The hot reload feature allows developers to instantly see changes in the UI, significantly speeding up the development process and improving productivity.

- **Cross-Platform Development:** With Flutter, you can write code once and run it on multiple platforms, including iOS, Android, web, and desktop. This single codebase approach not only saves time and resources but also ensures a unified user experience across all devices. Developing for multiple platforms simultaneously enhances development efficiency and reduces the overall project timeline.

- **Ready-to-Use Tools:** Flutter boasts a rich ecosystem of plugins and packages that simplify the integration of various functionalities, such as Firebase, payment gateways, and more. The comprehensive documentation and supportive community make it easier for developers to learn, troubleshoot, and stay updated with the latest advancements in Flutter.

- **Customization:** Flutter's flexible design capabilities allow for the creation of highly customized UIs with intricate animations. Developers can create custom widgets tailored to specific needs, ensuring the app’s appearance and behavior align perfectly with the desired user experience.

## How We Designed the ILA Portal UI

While developing the ILA Portal, we paid attention to ensuring a native feel on both iOS and Android devices. We continued the design principles of ILA, already established on the website, to maintain consistency. To achieve this, we carefully built custom widgets, custom themes, and custom text styles throughout the app. This approach ensured a seamless and cohesive user experience that aligns with the ILA brand identity.

- **SVG for High-Quality Visuals:** To ensure high-quality visuals across all devices, we utilized SVG for all in-app images and icons. This decision guaranteed that users consistently enjoy sharp and clear graphics, regardless of their device's resolution.

- **Hero Widget Animations:** We seamlessly integrated Hero widget animation in the app to enhance the user experience and provide a superior feel. The animation not only improved the aesthetic appeal but also made interactions more engaging.

- **Custom AppBar:** To enhance the UI on pages, we implemented a custom AppBar, providing a more cohesive and user-friendly experience.

- **Google Maps Customization:** We customized Google Maps features to align with the ILA color palette and ensured easy navigation by enabling users to click on corresponding call marks on the map.

- **Custom Widgets for Enhanced User Experience:** We developed custom widgets to enhance the user experience and maintain easily scalable code. Here's an example of a custom save button:

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../../utils/theme/color_constants.dart';
import '../state_data.dart';
import 'loaders/small_loader.dart';

class GlobalSaveButton extends StatefulWidget {
  final VoidCallback onTap;
  final bool? isLoading;
  final String? text;
  final bool? color;
  final Color? redColor;
  final Color? tappedRedColor;

  GlobalSaveButton({
    required this.onTap,
    this.isLoading,
    this.text,
    this.color,
    this.redColor,
    this.tappedRedColor,
  });

  @override
  _GlobalSaveButtonState createState() => _GlobalSaveButtonState();
}

class _GlobalSaveButtonState extends State<GlobalSaveButton> {
  late Color _buttonColor;

  @override
  void initState() {
    super.initState();
    _buttonColor = widget.redColor ?? ColorConstants.ilaColor;
  }

  void _onTapDown(TapDownDetails details) {
    setState(() {
      _buttonColor = widget.tappedRedColor ?? ColorConstants.ilaTappedColor;
    });
  }

  void _onTapUp(TapUpDetails details) {
    setState(() {
      _buttonColor = widget.redColor ?? ColorConstants.ilaColor;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Material(
      borderRadius: BorderRadius.circular(13),
      clipBehavior: Clip.antiAlias,
      child: InkWell(
        onTapDown: _onTapDown,
        onTapUp: _onTapUp,
        onTapCancel: () {
          setState(() {
            _buttonColor = widget.redColor ?? ColorConstants.ilaColor;
          });
        },
        onTap: widget.onTap,
        child: Container(
          height: 48,
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(13),
            color: widget.color == null
                ? _buttonColor
                : Provider.of<StateData>(context, listen: false).callsApplied
                    ? const Color(0xFF66BBB9)
                    : _buttonColor,
          ),
          child: Center(
            child: Padding(
              padding: const EdgeInsets.symmetric(vertical: 15, horizontal: 25),
              child: widget.isLoading == null
                  ? Text(
                      widget.text ?? "Save",
                      style: Theme.of(context)
                          .textTheme
                          .ILAHeadline1
                          .copyWith(color: Colors.white),
                    )
                  : Center(
                      child: widget.isLoading!
                          ? const SecondarySmallLoader()
                          : Text(
                              widget.text ?? "Save",
                              style: Theme.of(context)
                                  .textTheme
                                  .ILAHeadline1
                                  .copyWith(color: Colors.white),
                            ),
                    ),
            ),
          ),
        ),
      ),
    );
  }
}
```

- **Custom Date Picker Widget:** To ensure a native feel on both iOS and Android devices, we created a custom widget for picking the date of birth. This approach allows the app to offer a seamless and intuitive user experience that aligns with the platform's design principles. This is the one of the many example of our custom widgets.

The simplified code for the custom date picker widget:

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class DatePickerWidget extends StatefulWidget {
  const DatePickerWidget({Key? key}) : super(key: key);

  @override
  _DatePickerWidgetState createState() => _DatePickerWidgetState();
}

class _DatePickerWidgetState extends State<DatePickerWidget> {
  DateTime selectedDate = DateTime.now();

  void _selectDate(BuildContext context) async {
    if (Theme.of(context).platform == TargetPlatform.iOS) {
      showCupertinoModalPopup(
        context: context,
        builder: (_) => Container(
          height: 250,
          color: Colors.white,
          child: Column(
            children: [
              SizedBox(
                height: 200,
                child: CupertinoDatePicker(
                  mode: CupertinoDatePickerMode.date,
                  initialDateTime: selectedDate,
                  onDateTimeChanged: (val) {
                    setState(() {
                      selectedDate = val;
                    });
                  },
                ),
              ),
              CupertinoButton(
                child: Text('Done'),
                onPressed: () => Navigator.of(context).pop(),
              )
            ],
          ),
        ),
      );
    } else {
      DateTime? picked = await showDatePicker(
        context: context,
        initialDate: selectedDate,
        firstDate: DateTime(1930),
        lastDate: DateTime.now(),
      );
      if (picked != null && picked != selectedDate)
        setState(() {
          selectedDate = picked;
        });
    }
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () => _selectDate(context),
      child: Container(
        padding: EdgeInsets.symmetric(vertical: 15, horizontal: 25),
        decoration: BoxDecoration(
          border: Border.all(color: Colors.grey),
          borderRadius: BorderRadius.circular(5),
        ),
        child: Text(
          "${selectedDate.toLocal()}".split(' ')[0],
          style: TextStyle(fontSize: 16),
        ),
      ),
    );
  }
}
```

- **Custom Text Style:** To maintain consistency between the app and the website, we created a custom text style:

```dart
import 'package:flutter/material.dart';

extension ILATextTheme on TextTheme {
  TextStyle get ILAHeadline1 => const TextStyle(
      fontFamily: "ProximaNova-Regular.otf",
      fontWeight: FontWeight.w700,
      color: Color(0xFF000000),
      fontSize: 15);


  TextStyle get ILASubtitle1 => const TextStyle(
      fontFamily: "ProximaNova-Regular.otf",
      fontWeight: FontWeight.w400,
      color: Color(0xFF000000),
      fontSize: 13);

  }
```

- **Padding for Pixel-Perfect Design:** We defined padding values to ensure pixel-perfect and dynamic solutions for the app:

```dart
import 'package:flutter/material.dart';

extension ContextExtension on BuildContext {
  double dynamicWidth(double val) => MediaQuery.of(this).size.width * val;
  double dynamicHeight(double val) => MediaQuery.of(this).size.height * val;
}

extension NumberExtension on BuildContext {
  double get lowValue => dynamicHeight(0.01);
  double get mediumValue => dynamicHeight(0.03);
  double get ilaPadding => 25;
}

extension PaddingExtension on BuildContext {
  EdgeInsets get paddingAllLow => EdgeInsets.all(lowValue);
}

extension PagePaddingExtension on BuildContext {
  EdgeInsets get PagePadding => EdgeInsets.symmetric(horizontal: ilaPadding);
}

extension StaticColumnPaddingExtension on BuildContext {
  EdgeInsets get StaticColumnPadding => EdgeInsets.fromLTRB(25, 40, 25, 40);
}

extension EmptyWidget on BuildContext {
  Widget get emptyWidgetHeight => SizedBox(
        height: lowValue,
      );
}
```

- **Dark Mode Feature:** To accommodate user preferences and ensure optimal viewing conditions in various environments, the ILA mobile app offers both dark mode and light mode options. Users can seamlessly switch between these modes to suit their individual needs:

```dart
import 'package:flutter/material.dart';

class AppColors {
  static final Map<String, Color> lightColors = {
    "backgroundColor": const Color(0xffFAFBFE),
    "secondaryBackgroundColor": const Color(0xffFFFFFF),
    "headerColor": const Color(0xffFFFFFF),
    "authTextFieldColor": const Color(0xffF1F1F1),
    "whiteToBlack": Colors.white,
    "textColor": Colors.black,
    "secondaryTextColor": const Color(0xff282828),
    "thirdTextColor": const Color(0XFF282828),
    "ilaColor": const Color(0xff008D8A),
    "greenColor": const Color(0xff008000),
    "coordinatorContainerColor": const Color(0xff103B9D),
    "partnerContainerColor": const Color(0xffEB9D00),
    "undefinedTextColor": const Color(0xffFF0000),
    "bottomNavigationBarIconColor": Colors.black,
    "ilaContainerColor": Colors.white,
    "borderColor": Color(0xFFEBEBEB),
    "menuItemsBorderColor": const Color(0XFFEBEBEB),
    "iconBorderColor": const Color(0XFFEBEBEB),
    "headerBorderColor": const Color(0XFFEBEB

EB),
    "activityFlowBorder": const Color(0XFFEFF2F5),
    "navigationBarColor": const Color(0XFFE2E2E2),
    "saveButtonColor": const Color(0xFFF5F8FA),
  };

  static final Map<String, Color> darkColors = {
    "backgroundColor": const Color(0xff000000),
    "secondaryBackgroundColor": const Color(0xff000000),
    "headerColor": const Color(0xff212020),
    "authTextFieldColor": const Color(0xffc2bfbf),
    "whiteToBlack": Colors.black,
    "textColor": Colors.white,
    "secondaryTextColor": const Color(0xffc2bfbf),
    "thirdTextColor": const Color(0xffA0A0A0),
    "ilaColor": const Color(0xff008D8A),
    "greenColor": const Color(0xff008000),
    "coordinatorContainerColor": const Color(0xff103B9D),
    "partnerContainerColor": const Color(0xffEB9D00),
    "grayColor": const Color(0xffA0A0A0),
    "undefinedTextColor": const Color(0xffFF0000),
    "bottomNavigationBarIconColor": Colors.white,
    "ilaContainerColor": Colors.black,
    "borderColor": Color(0xffA0A0A0),
    "menuItemsBorderColor": const Color(0xffA0A0A0),
    "iconBorderColor": const Color(0xff151515),
    "headerBorderColor": const Color(0xff151515),
    "activityFlowBorder": const Color(0XFFEFF2F5),
    "navigationBarColor": const Color(0xff212020),
    "saveButtonColor": const Color(0xff212020),
  };

  static Map<String, Color> getColor(bool isDarkMode) {
    return isDarkMode ? darkColors : lightColors;
  }
}
```

## How did we monitor and manage tasks?

As the ILA mobile app team, we were aware of the significance of employing a project management tool for effective communication, collaboration, and project oversight. Embracing the Agile methodology, we conducted daily meetings to monitor tasks and devise solutions. Leveraging Jira as our project management tool, we organized tasks into sprints, adhering to the iterative development approach. Each sprint follows a set structure, including planning, development, testing, and review. This facilitated streamlined coordination, enhanced adaptability, and ensured the successful delivery of improvements within fixed timeframes.

![image](https://github.com/user-attachments/assets/b2e59cee-999e-4131-a974-9dfdc7df8dbb)


## How did we design the UI of the pages?

We utilized Figma as our design tool, meticulously crafting prototypes for each page of the application. Figma's comprehensive design capabilities provided us with detailed insights into the visual elements and user flow, enabling us to create pixel-perfect designs that seamlessly integrated with the Flutter codebase.

![image](https://github.com/user-attachments/assets/2a490a41-efdf-45a1-9b61-66d0f6af692c)

When designing the app's pages, we carefully considered the features and design details of the existing website, ensuring a cohesive user experience across both platforms. We also paid close attention to platform-specific features, incorporating Android features for Android devices and iOS features for iOS devices. This meticulous approach ensured a consistent and optimal user experience across different operating systems.

Beyond visual elements, we focused on creating an intuitive and user-friendly interface. We conducted extensive usability testing to identify potential pain points and refine the design accordingly. This iterative process ensured that the app was not only visually appealing but also easy to navigate and use.

By utilizing Figma's powerful design tools and adhering to platform-specific guidelines, we were able to create a mobile app that not only mirrored the website's design logic but also provided a unique user experience across both Android and iOS devices.🚀

#ILATech #Flutter #CrossPlatform #MobileAppDevelopment #InnovationInTech
