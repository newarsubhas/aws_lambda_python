LAMBDA_ROLE = arn:aws:iam::502075345842:role/MyLambdaExecution
FUNCTION_NAME = ResizeImage
AWS_REGION = ap-southeast-1
TIMEOUT = 15
MEMORY_SIZE = 128
ZIPFILE_NAME = image_resize.zip
HANDLER = handler.handle_resize

clean_pyc :
	find . | grep .pyc$ | xargs rm

install_deps :
	pip install -r requirements.txt -t .

build : install_deps clean_pyc
	zip $(ZIPFILE_NAME) -r *

create : build
	aws lambda create-function --region $(AWS_REGION) \
	                           --function-name $(FUNCTION_NAME) \
	                           --zip-file fileb://$(ZIPFILE_NAME) \
	                           --role $(LAMBDA_ROLE) \
	                           --description $(FUNCTION_NAME)_description \
	                           --handler $(HANDLER) \
	                           --runtime python2.7 \
	                           --timeout $(TIMEOUT) \
	                           --memory-size $(MEMORY_SIZE) \

update : build
	aws lambda update-function-code --region $(AWS_REGION) \
	                                --function-name $(FUNCTION_NAME) \
	                                --zip-file fileb://$(ZIPFILE_NAME) \
	                                --publish
delete:
	aws lambda delete-function \
	  --function-name $(FUNCTION_NAME)