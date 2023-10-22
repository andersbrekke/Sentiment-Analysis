# Sentiment-Analysis on Nords Goggles customer reviews:
This guide outlines a simple example of how to use Cohere to analyze customer sentiment in Google Business reviews. The reviews are from real customers of https://nordsgoggles.com/ and were posted publicly here: https://www.google.com/maps/place/Nords+Goggles/@46.423669,-129.9427085,3z/data=!4m8!3m7!1s0x888d8ada15234f13:0x28b6dc0736c4dd8f!8m2!3d46.423669!4d-129.9427086!9m1!1b1!16s%2Fg%2F11rd9gqnnq?entry=ttu

## Set-up
Install package with pip:
    
    pip install cohere

Import (using bearer from Cohere docs: https://docs.cohere.com/reference/sentiment-analysis)
  
    import cohere
    co = cohere.Client('LYalFjNMtWRZUwCT8EtTW9rBSMyUbMOJ9Xa8C5Mq')

### Define a few examples (generic product feedback)

    from cohere.responses.classify import Example
    examples=[
      Example("This product is great, I highly recommend it", "positive"),
      Example("Awesome stuff! Everyone should buy this", "positive"),
      Example("I will never buy this product again", "negative"),
      Example("This is probably the worst thing I've ever seen", "negative"),
      Example("You can use this product for different activities", "neutral"),
      Example("This is a product for swimming", "neutral"),
    ]

#### Add the inputs (reviews taken from the Nords Goggles Business profile: https://www.google.com/search?sca_esv=575648043&sxsrf=AM9HkKnyuMv7PWfTXntsuJOptwua79Kf_g%3A1698010812081&q=Nords%20Goggles&stick=H4sIAAAAAAAAAONgU1I1qLCwsEixSExJNDQ1MjZJMzS2MqgwskgyS0k2MDc2SzZJSbFIW8TK65dflFKs4J6fnp6TWgwAmol0hTkAAAA&mat=Cfif2mEUFNdt&ved=2ahUKEwjbmZ2az4qCAxWfDkQIHX4fBr0QrMcEegQICBAH)

    inputs=[
      "I place an order that I never received and I have been asking for a refund only to be told they would resend it and I never got and still no refund. Now they're closed",
      "recently stumbled upon this innovative brand. Love the color options and the delivery was quick and easy",
      "Couldn’t recommend these more! I loved being able to personalize my googles - they look great and also perform great.",
      "Great product. Love the price point. Went with the Borealis mirrored finish. Great for outdoor training. Would highly recommend."
    ]

##### Genererate Sentiment Analysis

    response = co.classify(
    model='large',
    inputs=inputs,
    examples=examples,
    )

    print(response.classifications)


###### Output and Business Value:

[Classification<prediction: "negative", confidence: 0.9694196, labels: {'negative': LabelPrediction(confidence=0.9694196), 'neutral': LabelPrediction(confidence=0.0067857974), 'positive': LabelPrediction(confidence=0.023794608)}>, Classification<prediction: "positive", confidence: 0.95937854, labels: {'negative': LabelPrediction(confidence=0.012157313), 'neutral': LabelPrediction(confidence=0.028464155), 'positive': LabelPrediction(confidence=0.95937854)}>, Classification<prediction: "positive", confidence: 0.9375513, labels: {'negative': LabelPrediction(confidence=0.01753235), 'neutral': LabelPrediction(confidence=0.044916306), 'positive': LabelPrediction(confidence=0.9375513)}>, Classification<prediction: "positive", confidence: 0.9148662, labels: {'negative': LabelPrediction(confidence=0.01203235), 'neutral': LabelPrediction(confidence=0.07310147), 'positive': LabelPrediction(confidence=0.9148662)}>]

Analysis: 
 - The first review is negative with a high confidence of 0.96, and the followoing three reviews are all positive with over 0.9 confidence scores

Being able to quickly detect and understand user sentiment allows companies to address customer concerns and improve their products and services.
