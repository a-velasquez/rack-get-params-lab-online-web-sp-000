# Rack Routes and GET Params

We've provided the code for a basic list of items. Now it's your turn to extend it.
Do you work in `app/application.rb`.

## Vocabulary Word: "Route"

In applications built on Rack, we use the noun "route" to to refer to a path
that the application has a special response to.

Thus the `/shoes` "route" shows information about shoes. The `/profile` "route"
shows information about the logged-in user. The `/logout` route does something
to delete some information that let the server know the user was logged in.

## Instructions

  1. Create a new class array called `@@cart` to hold any items in your cart
  2. Create a new route called `/cart` to show the items in your cart
  3. Create a new route called `/add` that takes in a `GET` param with the key `item`. This should check to see if that item is in `@@items` and then add it to the cart if it is. Otherwise give an error

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/rack-get-params-lab' title='Rack Routes and GET Params'>Rack Routes and GET Params</a> on Learn.co and start learning to code for free.</p>
```ruby
class Application

  @@items = ["Apples","Carrots","Pears"]
  @@cart = []

  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)

    if req.path.match(/items/)
      @@items.each do |item|
        resp.write "#{item}\n"
      end
    elsif req.path.match(/search/)
      search_term = req.params["q"]
      resp.write handle_search(search_term)
    elsif req.path.match(/add/)
      item = req.params["item"]
      resp.write add_to_cart(item)
    elsif req.path.match(/cart/)  
      if @@cart.empty?
        resp.write "Your cart is empty"
      else
        @@cart.each {|item| resp.write  "#{item}\n"}
      end
    else
      resp.write "Path Not Found"
    end

    resp.finish
  end

  def add_to_cart(item)
    if @@items.include? item
      @@cart << item
      return "added #{item}"
    else
      return "We don't have that item"
    end
  end



  def handle_search(search_term)
    if @@items.include?(search_term)
      return "#{search_term} is one of our items"
    else
      return "Couldn't find #{search_term}"
    end
  end
end
