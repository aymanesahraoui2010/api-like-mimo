const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp();

// API لزيادة عدد اللايكات
exports.addLike = functions.https.onRequest((req, res) => {
  const postId = req.query.postId;
  if (!postId) {
    return res.status(400).send('postId is required');
  }

  const postRef = admin.firestore().collection('posts').doc(postId);
  postRef.update({
    likes: admin.firestore.FieldValue.increment(1)
  })
  .then(() => res.status(200).send('Like added successfully'))
  .catch((error) => res.status(500).send(error.message));
});
