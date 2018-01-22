# Find-Video-Length-React-Native
A workaround to find the video length/duration from react-native-image-picker package

In my recent project I was stuck when the client asked me to set the timelimit of a video. I thought it would be easy, I would just call a built-in function and boom! It would work! But unfortunately I could not find such a function to call. 

First of all, to take a video I was using a package called [react-native-image-picker](https://github.com/react-community/react-native-image-picker), that package has the built in option to set the duration limit of a video while recording a video which is great. But unfortunately it does not have any such option to set the timelimit while a user is selecting and uploading a video from the device gallery. That's where I was stuck.

Then I found out another great package for react-native which gives the meta-data of any media file. The package is called [react-native-media-meta](https://github.com/mybigday/react-native-media-meta). I just have to give the filepath to the package and it would return me meta-data of that particular media file. So everything becomes simple for me now. I tried to do the following:

1. Get the file path of the media from react-native-image-picker option.
2. Sending the path to the meta-data package.
3. Getting back the meta-data which also includes video duration.
4. Finally using condition to do whatever I want to do.

But then again I was stuck! react-native-image-picker gives back the file path only for the android but not for the ios. So it was only working for android. Eventually I found out the I have to modify the uri substring to get the actual path for ios. Finally I was able to find out the video length using react-native-image picker. 

# The working sample of code is given below:

```
// react-native-image-picker options
const options = {
  title: '', // specify null or empty string to remove the title
  cancelButtonTitle: 'Cancel',
  takePhotoButtonTitle: 'Record Video', // specify null or empty string to remove this button
  chooseFromLibraryButtonTitle: 'Upload Video', // specify null or empty string to remove this button
  cameraType: 'back', // 'front' or 'back'
  thumbnail: true,
  durationLimit: 300, // 5 mins
  allowsEditing: true,
  mediaType: 'video', // 'photo' or 'video'
  videoQuality: 'high', // 'low', 'medium', or 'high'
  storageOptions: { // if this key is provided, the image will get saved in the documents/pictures directory (rather than a temporary directory)
    skipBackup: true, // image will NOT be backed up to icloud
    path: 'images' // will save image at /Documents/images rather than the root
  }
};
ImagePicker.showImagePicker(options, (video) => {
      const path = video.path; // for android
      const path = video.uri.substring(7) // for ios
      const maxTime = 300000; // 5 min
      MediaMeta.get(path)
        .then((metadata) => {
          if (metadata.duration > maxTime ) {
            Alert.alert(
              'Sorry',
              'Video duration must be less then 5 minutes',
              [
                { text: 'OK', onPress: () => console.log('OK Pressed') }
              ],
              { cancelable: false }
            );
          } else {
            // Upload or do something else
          }
        }).catch(err => console.error(err));
    });
```

If there is any better option to do this, please create an issue and let me know. :)
