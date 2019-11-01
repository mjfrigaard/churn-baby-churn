library(tidyverse)
library(lubridate)
library(ggthemes)
library(gmodels)

# read data
telco <- read.csv("WA_Fn-UseC_-Telco-Customer-Churn.csv") %>% janitor::clean_names(case = "big_camel")
telco %>% glimpse(78)

# convert churn value to numeric
telco$Churn_numeric <- ifelse(telco$Churn == "Yes", 1, 0)

# churn rate total format
total_churn <- telco %>%
  group_by(Churn_numeric) %>% 
  summarise(total_sample = n()) %>%
  mutate(rate = total_sample / sum(total_sample))

# viz total churn rate
ggRententionTotal <- total_churn %>%
  ggplot(aes(x = reorder(Churn_numeric, + rate), y = rate)) +
  stat_summary(fun.y=mean, geom="bar", fill = "#0072B2") + 
  stat_summary(fun.data=mean_cl_boot, geom="errorbar", width=0.3) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_x_discrete(labels=c("1" = "Churned", "0" = "Retained")) +
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  theme(legend.position = 'none') +
  labs(title = "Total Sample Retention",
       x = "Outcome", y = "Percent")
  ggRententionTotal

# churn rate format per paying number of months with the company
churn_tenure <- telco %>%
  group_by(Tenure) %>%
  mutate(sample = n()) %>%
  filter(Churn_numeric == 1) %>%
  #group_by(Tenure) %>%
  summarize(churned = sum(Churn_numeric), 
            sample = first(sample),
            churn_rate = round(churned/sample,4)) %>%
  group_by(Tenure) %>%
  mutate(churn_rate_cum = cumsum(churn_rate))

# viz total churn by paying months
ggplot(churn_tenure, aes(Tenure, churn_rate)) +
  geom_point() +
  geom_smooth() +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  labs(title = "Churn Rate
By Months Retained",
       subtitle = "Percentage of customers that churned per 
number of paying months with the company ",
       x = "Paid Months", y = "Percent Churned")


# gender format
churn_frequencies <- telco %>%
  group_by(Gender) %>%
  mutate(sample = n()) %>%
  filter(Churn_numeric == 1) %>%
  group_by(Gender) %>%
  summarise(churned = sum(Churn_numeric),
            sample = first(sample),
            churned = round(churned/sample,3),
            retained = round(1 - churned,3)) %>%
  group_by(Gender)

# gender 
ggRententionByGender <- telco %>%
  ggplot(aes(x = reorder(Gender, + Churn_numeric), y = Churn_numeric)) + 
  stat_summary(fun.y=mean, geom="bar", fill = "#0072B2") + 
  stat_summary(fun.data=mean_cl_boot, geom="errorbar", width=0.3) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  theme(legend.position = 'none') +
  labs(title = "Churn Rate By Gender",
       x = "Gender", y = "Percent Churned")
ggRententionByGender
  

# senior citizens
ggRententionBySeniors <- telco %>%
  ggplot(aes(x = reorder(SeniorCitizen, + Churn_numeric), y = Churn_numeric)) + 
  stat_summary(fun.y=mean, geom="bar", fill = "#0072B2") + 
  stat_summary(fun.data=mean_cl_boot, geom="errorbar", width=0.3) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_x_discrete(labels=c("1" = "SeniorCitizen", "0" = "Not Senior Citizen")) +
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  theme(legend.position = 'none') +
  labs(title = "Churn Rate By Senior Citizen",
       x = "Senior Citizen", y = "Percent Churned")
ggRententionBySeniors

# payment method
ggRententionByPayment <- telco %>%
  ggplot(aes(x = reorder(SeniorCitizen, + Churn_numeric), y = Churn_numeric)) + 
  stat_summary(fun.y=mean, geom="bar", fill = "#0072B2") + 
  stat_summary(fun.data=mean_cl_boot, geom="errorbar", width=0.3) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_x_discrete(labels=c("1" = "Senior", "0" = "Non-Senior")) +
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  facet_grid(~ PaymentMethod) +
  theme(legend.position = 'none') +
  labs(title = "Churn Rate By Payment Method",
       x = "Senior Citizen", y = "Percent Churned")
ggRententionByPayment



# round monthly payment to whole dollars
telco$MonthlyCharges_round <- round(telco$MonthlyCharges,0)

# find quantiles
quantile(telco$MonthlyCharges_round)

# create quantiles
telco$MonthlyCharges_quantiles <- ifelse(telco$MonthlyCharges_round <= 36, "25 Percentile", 
                                         ifelse(telco$MonthlyCharges_round > 36 & telco$MonthlyCharges_round <= 70, "50 Percentile",
                                                ifelse(telco$MonthlyCharges_round > 70 & telco$MonthlyCharges_round <= 90, "75 Percentile",
                                                       ifelse(telco$MonthlyCharges_round > 90, "100 Percentile", "other"))))

# churn rate format per monthly charges
churn_monthlypay <- telco %>%
  group_by(MonthlyCharges_round) %>%
  mutate(sample = n()) %>%
  filter(Churn_numeric == 1) %>%
  group_by(MonthlyCharges_round, MonthlyCharges_quantiles ) %>%
  summarize(churned = sum(Churn_numeric), 
            sample = first(sample),
            churn_rate = round(churned/sample,4)) %>%
  group_by(MonthlyCharges_round, MonthlyCharges_quantiles) 

# viz total churn by monthly payments
ggplot(churn_monthlypay, aes(MonthlyCharges_round, churn_rate)) +
  geom_point() +
  geom_smooth(method = "lm") +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  facet_grid(~ MonthlyCharges_quantiles, scales = "free") +
  labs(title = "Churn Rate
By Monthy Payment",
       subtitle = "Percentage of customers that churned by 
the customer's monthly payment",
       x = "Monthly Payment", y = "Percent Churned")

# round total charges
telco$TotalCharges_round <- round(telco$TotalCharges,0)

# churn rate format per total charges
churn_totalpay <- telco %>%
  group_by(TotalCharges_round) %>%
  mutate(sample = n()) %>%
  filter(Churn_numeric == 1) %>%
  group_by(TotalCharges_round , PaymentMethod) %>%
  summarize(churned = sum(Churn_numeric), 
            sample = first(sample),
            churn_rate = round(churned/sample,4)) %>%
  group_by(TotalCharges_round, PaymentMethod) 

# viz total churn by total charges
ggplot(churn_totalpay, aes(TotalCharges_round, churn_rate)) +
  geom_point() +
  geom_smooth(method = "lm") +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  facet_grid(~ PaymentMethod) +
  labs(title = "Churn Rate
By Total Charges",
       subtitle = "Percentage of customers that churned by 
the customer's total charges",
       x = "Total Charges", y = "Percent Churned")


# paperless billing

labels <- c(Yes = "With Paperless Billing", No = "Without Paperless Billing")

ggRententionByInternet <- telco %>%
  ggplot(aes(x = reorder(InternetService, + Churn_numeric), y = Churn_numeric)) + 
  stat_summary(fun.y=mean, geom="bar", fill = "#0072B2") + 
  stat_summary(fun.data=mean_cl_boot, geom="errorbar", width=0.3) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) + 
  scale_y_continuous(labels = scales::percent_format(accuracy = 1)) +
  facet_wrap(~ PaperlessBilling, labeller=labeller(PaperlessBilling = labels)) +
  theme(legend.position = 'none') +
  labs(title = "Churn Rate By Internet Service
and Paperless Billing",
       x = "Phone Service Type", y = "Percent Churned")
ggRententionByInternet

CrossTable(telco$PaperlessBilling, telco$Churn_numeric, format = "SPSS", fisher = TRUE)
