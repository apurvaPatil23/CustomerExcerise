# CustomerExcerise
1.	Provide a way to get a product’s total number of customer reviews whose ratings are within a given range (inclusive).

DefaultCustomerReviewService.java

public List<CustomerReviewModel> getReviewsByRating(Keys k)
{
				
List<CustomerReview> reviewsByRating = CustomerReviewManger.getInstance().getAllReviewsByRating(
						(Product)getModelService().getSource(product));
return (List)getModelService().getAll(reviews, new ArrayList());
}

DefaultCustomerReviewDao.java

public List<CustomerReviewModel> getReviewsByRating (Keys k)
{
String query = "SELECT {" + Item.PK + "} FROM {" + "CustomerReview" + "} WHERE {" + "rating" + "}=?rating BETWEEN {"+"k.MINIMAL_RATING"+"} AND {"+"k.MAXIMAL_RATING"+"} ORDER BY {" + "creationtime" + "} DESC";

FlexibleSearchQuery fsQuery = new FlexibleSearchQuery(query);
				fsQuery.addQueryParameter("rating", rating);
fsQuery.setResultClassList(Collections.singletonList(CustomerReviewModel.class));
SearchResult<CustomerReviewModel> searchResult = getFlexibleSearchService().search(fsQuery);
				return searchResult.getResult();
				
			}

CustomerReviewManager.java

public List<CustomerReview> getReviewsByRating(Keys k)
{
return getReviewsByRating(JaloSession.getCurrentSession().getSessionContext(), k);
}
CustomerReviewService.java

public abstract List<CustomerReviewModel> getReviewsByRating(Keys k);

2.	Add the following additional checks before creating a customer review:

a.	Your service should read a list of curse words. This list should not be defined in Java class. 

checkMandatoryAttribute("cursewords",allAttributes, missing); 

b.	Check if Customer’s comment contains any of these curse words. If it does, throw an exception with a message.

if(curseList.contains(allAtributes))
{
throw new JaloInvalidParameterException("invalid "+ missing +" for creating a new CustomerReview",0);
}
c.	Check if the rating is not < 0.  If it is < 0, throw an exception with a message.

if(rating<0)
{
throw new JaloInvalidParameterException("Rating Invalid");
}

If all the rules are passed, go ahead and create the customer review.

3.	Ensure the new functionality can be used elsewhere in the application (i.e.  a bean containing the new functionality is defined within the customerreview-spring.xml).

CustomerReviewSystemSetup.java

public CurseWordsRestrictionService getCurseWordsRestriction()
			{
				return this.curseWordsRestrictionService;
			}
			
@Required
public void setCurseWordsRestrictionService(CurseWordsRestrictionService curseWordsRestrictionService)
			{
		this.curseWordsRestrictionService = curseWordsRestrictionService;
			}

Customerreview-spring.xml

<property name="curseWordsRestrictionService"ref="curseWordsRestrictionService"/>
