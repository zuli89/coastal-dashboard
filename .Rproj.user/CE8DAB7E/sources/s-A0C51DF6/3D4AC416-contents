library(dataRetrieval)
library(dplyr)
library(tidyr)
library(lubridate)

sites<- c("08067252", "08162501", "08188810")

start.date<- "2013-08-01"
end.date<- Sys.Date()

Q<- readNWISuv(siteNumbers= sites, 
                 parameterCd= "00060", #Discharge
                 startDate = start.date,
                 endDate = end.date)


Q$date <- as.Date(Q$dateTime)
Q$time <- format(Q$dateTime,"%H:%M:%S")

Q1<-separate(Q, time, c("H", "M", "S")) #separates time into Hours, min, sec

Q_hour<- Q1 %>%
  filter(M == "00") %>% #obtain only horly data
  select("site_no", "dateTime", "date", "X_00060_00000", "X_00060_00000_cd")

write.csv(Q_hour, file = "discharge.csv", row.names = FALSE) #create streamflow dataset


#Download data for collected samples

parm<-c("00061", "00010", "00095", "00300", "00400", "63680", "00600", "00605", "00607", "00608", "00618", "00613","00625", "00665", "00660", "00671", "00680", "00681", "62855", "62854", "70331", "80154")

QWdata<- readNWISqw(siteNumbers= sites, 
                  parameterCd= parm, 
                  startDate = start.date,
                  endDate = end.date, reshape = TRUE)


#filter out only sampl dates and times

QWdates<- QWdata[c(2:4,19)] 
#rounds the time to the nearest hour so it can be matched to Q hourly data
QWdates$dateTime <- round_date(QWdates$startDateTime, "hour")
#matches Hourly data and returns streamflow


QWpoints<- left_join(QWdates, Q_hour, by=c("site_no", "dateTime"))
QWpoints<- QWpoints[c(1:3,5,7:8)]

