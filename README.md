# app-review
Frist Import the Play Core Library into your project: Add the following in your app’s build.gradle file:
dependencies {
    // This dependency is downloaded from the Google’s Maven repository.
    // So, make sure you also include that repository in your project's build.gradle file.
    implementation 'com.google.android.play:core:1.8.2'

    // For Kotlin users also import the Kotlin extensions library for Play Core:
    implementation 'com.google.android.play:core-ktx:1.8.1'
}
Create the ReviewManager instance
// If using kotlin: 
val manager = ReviewManagerFactory.create(context)

// If using Java:
ReviewManager manager = ReviewManagerFactory.create(context)
Request a ReviewInfo object
// If using kotlin: 
val request = manager.requestReviewFlow()
request.addOnCompleteListener { request ->
    if (request.isSuccessful) {
        // We got the ReviewInfo object
        val reviewInfo = request.result
        val flow = manager.launchReviewFlow(activity, reviewInfo)
        flow.addOnCompleteListener { _ ->
            // The flow has finished. The API does not indicate whether the user
            // reviewed or not, or even whether the review dialog was shown. Thus, no
            // matter the result, we continue our app flow.
        }
    } else {
        // There was some problem, continue regardless of the result.
        // you can show your own rate dialog alert and redirect user to your app page
        // on play store.
    }
}

// If using Java
ReviewManager manager = ReviewManagerFactory.create(this);
Task<ReviewInfo> request = manager.requestReviewFlow();
request.addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        // We can get the ReviewInfo object
        ReviewInfo reviewInfo = task.getResult();
        Task<Void> flow = manager.launchReviewFlow(activity, reviewInfo);
        flow.addOnCompleteListener(task -> {
            // The flow has finished. The API does not indicate whether the user
            // reviewed or not, or even whether the review dialog was shown. Thus, no
            // matter the result, we continue our app flow.
        });
    } else {
        // There was some problem, continue regardless of the result.
        // you can show your own rate dialog alert and redirect user to your app page
        // on play store.
    }
});
