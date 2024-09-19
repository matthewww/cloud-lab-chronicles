https://aws.amazon.com/blogs/machine-learning/revolutionize-logo-design-creation-with-amazon-bedrock-embracing-generative-art-dynamic-logos-and-ai-collaboration/

This architecture workflow involves the following steps:

1. In the frontend UI, a user chooses from one of two options to get started:
- Generate an initial image.
- Provide an initial image link.
2. The user provides a text prompt to edit the given image.
3. The user chooses Call API to invoke API Gateway to begin processing on the backend.
4. The API invokes a Lambda function, which uses the Amazon Bedrock API to invoke the Stability AI SDXL 1.0 model.
5. The invoked model generates an image, and the output image is stored in an Amazon Simple Storage Service (Amazon S3) bucket.
6. The backend services return the output image to the frontend UI.
7. The user can use this generated image as a reference image and edit it, generate a new image, or provide a different initial image. They can continue this process until the model produces a satisfactory output.
