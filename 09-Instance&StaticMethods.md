# Instance and Static Methods

## Instance
It is method on a single document.You can write any function which you can call and run on the document.
<!-- //instance method syntax -->
schema.methods.methodName = (){
    return this.something
}
//this here represents the document
ex:-
<!-- instance method -->
userSchema.methods.checkEmailDomain = function () {
  return this.email.split('@')[1];
};

## Static method
It is a method on the model itself.
<!-- static method syntax -->
schema.statics.methodName(){
    return this.find({something:something})
}
//this here represents the model
ex:-
<!-- static method -->
schema.statics.findActiveUsers = function (){
    return this.find({isActive:true})
}