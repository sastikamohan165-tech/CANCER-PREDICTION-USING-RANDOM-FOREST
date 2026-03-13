# Install packages
install.packages("mlbench")
install.packages("randomForest")

# Load libraries
library(mlbench)
library(randomForest)

# Load cancer dataset
data(BreastCancer)

# Store dataset
cancer <- BreastCancer

# Remove ID column
cancer$Id <- NULL

# Remove missing values
cancer <- na.omit(cancer)

# Split dataset
set.seed(123)
index <- sample(1:nrow(cancer), 0.7*nrow(cancer))

train <- cancer[index, ]
test <- cancer[-index, ]

# Train Random Forest model
model <- randomForest(Class ~ ., data=train, ntree=100)

# Predict test data
prediction <- predict(model, test)

# Confusion matrix
result <- table(Actual=test$Class, Predicted=prediction)
print(result)

# Accuracy calculation
accuracy <- sum(diag(result)) / sum(result)
cat("Accuracy =", accuracy)
