```js

/*======== ======== ======== */
/* ======== CLIENT ======== */
/*======== ======== ======== */

function() {
	/* widget controller */
	var c = this;
	var calendarEvents = c.data.calendarEvents;
	
	//console.log(c.data.calendarEvents);

	calendarEvents = calendarEvents.map(function(el){
		el.start 	= moment(el.start_display,'DD-MM-YYYY HH:mm:ss').format("YYYY-MM-DD HH:mm:ss");
		el.end 		= moment(el.end_display,'DD-MM-YYYY HH:mm:ss').format("YYYY-MM-DD HH:mm:ss");
		el.day  	= moment(el.start_display,'DD-MM-YYYY HH:mm:ss').format("DD");
		el.start_hour = moment(el.start_display,'DD-MM-YYYY HH:mm:ss').format("HH:mm");
		el.end_hour = moment(el.end_display,'DD-MM-YYYY HH:mm:ss').format("HH:mm");
		return el;
	});

	//console.log(calendarEvents);
	
	$('#calendar').fullCalendar({
		// put your options and callbacks here
		initialView: 'dayGridMonth',
		initialDate: '2020-11-12',
		/*eventColor: 'green',*/
		events: c.data.calendarEvents,
		eventRender: function(eventObj, $el) {
			//console.log(eventObj.conflict);
			//modal com as informações
			var content = 
					"Data inicio: " + eventObj.start_display + 
					" \n Data fim: " + eventObj.end_display + 
					(eventObj.ic != "" ? " \n IC:" + eventObj.ic : "") + 
					(eventObj.time_solicitante != "" ? " \n Time Solicitante:" + eventObj.time_solicitante : "") + 
					(eventObj.owner_change != "" ? "\n Dono da change:" + eventObj.owner_change : "") + 
					(eventObj.conflict ? "\n Conflito:" + eventObj.conflict.number : "");
			$el.popover({
				title: eventObj.title,
				content: content,
				trigger: 'hover',
				placement: 'top',
				container: 'body'
			});
		}
	});

	c.data.getMonth = function(ev){

		var month = moment(ev.start).format("M");
		var b = "";

		switch(parseInt(month)){
			case 1: b = "Janeiro";
				break;
			case 2: b = "Fevereiro";
				break;
			case 3: b = "Março";
				break;
			case 4: b = "Abril";
				break;
			case 5: b = "Maio";
				break;
			case 6: b = "Junho"; 
				break;
			case 7: b = "Julho";
				break;
			case 8: b = "Agosto";
				break;
			case 9: b = "Setembro";
				break;
			case 10: b = "Outubro";
				break;
			case 11: b = "Novembro";
				break;
			case 12: b = "Dezembro";
				break;
		}
		return b;
	}


	//console.log(c.data.calendarEvents);
}


/*======== ======== ======== */
/* ======== CSS ======== */
/*======== ======== ======== */


@import url('https://fonts.googleapis.com/css?family=Abril+Fatface');
@import url('https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@300;400&amp;display=swap');



h3,h2,p{
  color: #000;
}

label{
  font-weight: 500;
}

p{
  font-family: 'Roboto', 'Helvetica';
  font-size: 14px;
}

h3{
  font-family: 'Roboto Slab', serif; 
  font-weight: 300;
  font-size: 2.3em;
}

.events-title{
  h3{
    margin-top: 50px;
    margin-bottom: 40px;
    line-height: 2.625rem;
    position: relative;
    padding-bottom: 23px;
  } 

}

.events-title h3:after {
  content: "";
  position: absolute;
  width: 28px;
  height: 4px;
  background-color: #ffc709;
  left: 0;
  bottom: 0;
}

.section-resume-events{
  padding: 45px 5px 0;
  input{
    padding: 10px;
    width: 100%;
    background-color: #fff;
    padding: 20px 48px 20px;
    /* position: relative; */
    display: flex;
    color: #000;
    border: solid 1px #ececec;
    /*border-radius: 4px;
    box-shadow: 2px 2px 11px 3px #efefef;*/
    flex-direction: column;
  }

  .lupa-busca{
    i{
      position:absolute;
      top: 66px;
      left: 42px;
    }	
  }

  .list-events{
    overflow-y: scroll;
    max-height: 510px;
    overflow: auto;
    small{
      color: #696767;
      font-size: 14px;
    }
  }
}


.feature-event{
  padding: 0 0 3px 0px;
  float: left;
  min-width: 300px;
  border-bottom: 1px solid #ddd;


  .feature-day{
    text-align:center!important;
    float: left;
    clear: none;
    text-align: inherit;
    margin-left: 0;
    padding-right: 1rem;
    p{
      font-size: 35px;
      line-height: 0.5;
      padding-top: 45px;
    }

  }

  .resume-event{
    clear: none;
    width: 100%;
    padding: 35px 0 0 0;
    text-align: inherit;
    margin-left: 0;
    p{
      font-size: 17px;
      font-weight: 500;
    }
  }

}


ol {
  list-style: none;
  margin: 0;
  padding: 0;
}
ol li {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  margin: 5px 5px;
  padding: 3px 17px;
  position: relative;
  border-bottom: 1px solid #ddd;
}
ol li::before {
  font-weight: bold;
  font-size: 3rem;
  margin-right: 0.5rem;
  font-family: 'Abril Fatface', serif;
  line-height: 1;
}

ol li:after{
  content: "";
  position: absolute;
  width: 11px;
  height: 11px;
  left: 0px;
  background-color: #d6d5d5;
  top: 8px;
  border-radius: 50%;
  z-index: 1;
}

.typeEvent{
  font-size: 12px;
  padding: 0;
  margin: 0;
  color: #696767;
}


@media only screen and (max-width: 991px) {
  .feature-event{
    padding: 0 0 55px 0px;
    border-bottom: 1px solid #ddd;
    float: none;
  }
}

/*======== ======== ======== */
/* ======== SERVER ======== */
/*======== ======== ======== */

function() {
	/* populate the 'data' object */
	/* e.g., data.table = $sp.getValue('table'); */

	data.finnancialEventsColor = options.announcements_color || '#358dc5';
	data.tiEventsColor = options.ti_event_color || "blue";
	data.changesColor = options.changes_color || '#f47c01';
	data.changesColorConflict = options.changes_color_conflict || 'red';
	data.footerMessage = options.footer_message;
	data.vacationsColor = options.vacations_color || '#ddd';

	var mFunctions = {
		getEvents: function(){
			var events = [];

			//get all events type event_widget and is today or after//
			var grAnnouncement = new GlideRecord('announcement');
			grAnnouncement.addEncodedQuery("active=true^typeLIKEade089271b60645040718590f54bcb82^u_type_eventIN20,30");
			grAnnouncement.orderBy("from");
			grAnnouncement.query();
			while (grAnnouncement.next()){
				events.push({
					"number": grAnnouncement.getDisplayValue("number"),
					"title" : grAnnouncement.getDisplayValue("title"),
					"start" : grAnnouncement.getValue("from"),
					"end" : grAnnouncement.getValue("to"),
					"start_display" : grAnnouncement.getDisplayValue("from"),
					"end_display" : grAnnouncement.getDisplayValue("to"),
					"sys_id": grAnnouncement.getValue("sys_id"),
					//color of announcements
					"backgroundColor": this.getColor(grAnnouncement.getValue("u_type_event")),
					"ic": "",
					"time_solicitante" : "",
					"owner_change" : "",
					"type": "event",
					"subtype" : grAnnouncement.getDisplayValue("u_type_event")
					
				});
			}
			
			//gs.addErrorMessage(grAnnouncement.getDisplayValue("u_type_event"));

	
			//retorna todas as changes com a flag show_on_calendar = true e dentro dos ultimos 12 meses
			var grChange = new GlideRecord('change_request');
			grChange.addEncodedQuery("u_show_on_calendar=true^sys_created_onONLast 12 months@javascript:gs.beginningOfLast12Months()@javascript:gs.endOfLast12Months()");
			grChange.orderBy("from");
			grChange.query();
			while (grChange.next()) {

				var changeConflict = this.checkConflictChange(grChange.sys_id,grChange.start_date,grChange.end_date,grChange.cmdb_ci);
			
				//gs.addInfoMessage(JSON.stringify(changeConflict));
					
				events.push({
					"number": grChange.getDisplayValue("number"),
					"title" : grChange.getDisplayValue("number") + ' - ' + grChange.getDisplayValue("short_description"),
					"start" : grChange.getValue("start_date"),
					"end" 	: grChange.getValue("end_date"),
					"start_display" : grChange.getDisplayValue("start_date"),
					"end_display" : grChange.getDisplayValue("end_date"),
					"sys_id": grChange.getValue("sys_id"),
					"conflict" : changeConflict.change_info,
					//color of announcements
					"backgroundColor": changeConflict.hasConflict ? data.changesColorConflict : data.changesColor,
					"ic" : grChange.getDisplayValue("cmdb_ci"),
					"time_solicitante" : grChange.getDisplayValue("u_time_solicitante"),
					"owner_change" : grChange.getDisplayValue("opened_by"),
					"type": "change",
					"subType" : ""
				});
				
			}
			
			//gs.addInfoMessage(JSON.stringify(events));
			
			return events;
		},
		
		getColor : function(type){
			
			var color = ''
			if(type == 20)
				color = data.finnancialEventsColor;
			
			if(type == 30)
				color = data.tiEventsColor;
			
			if(type == 10)
				color = data.vacationsColor;
			
			return color;
		},

		checkConflictChange : function(sys_id,start,end,ic){

			var hasConflict = false;
			var changeInfo = {};
			/*
			var startDate = start.split(" ")[0];
			var startTime = start.split(" ")[1];

			var endDate = end.split(" ")[0];
			var endTime = end.split(" ")[1];
			*/

			var start_date = new GlideDateTime(start);
			var end_date = new GlideDateTime(end);

			var grGetChange = new GlideRecord('change_request');
			grGetChange.addEncodedQuery("sys_created_onONThis month@javascript:gs.beginningOfLast12Months()@javascript:gs.endOfThisMonth()^cmdb_ci="+ic+"^sys_id!="+sys_id);
			//grGetChange.setLimit(1);
			grGetChange.query();
			while (grGetChange.next()) {

				//gs.addInfoMessage(sys_id+":::"+ grGetChange.number +"-"+ grGetChange.getValue("start_date") +"-"+ grGetChange.getValue("end_date"));
				var change_start = new GlideDateTime(grGetChange.getValue("start_date"));
				var change_end = new GlideDateTime(grGetChange.getValue("end_date"));

				//verifica se o inicio é igual ou maior que o range atual
				//changeStart >= startDate && changeStart <= endDate???
				if(change_start.onOrAfter(start_date) && change_start.onOrBefore(end_date)){
					hasConflict = true;
					changeInfo = {
						"number": grGetChange.getDisplayValue("number"),
						"desc" : grGetChange.getDisplayValue("short_description")
					};

					//verifica se o fim é menor ou igual o range atual
					//changeEnd <= startDate && changeStart <= endDate???
				}else if(change_end.onOrBefore(end_date) && change_end.onOrAfter(start_date)){
					hasConflict = true;
					changeInfo = {
						"number": grGetChange.getDisplayValue("number"),
						"desc" : grGetChange.getDisplayValue("short_description")
					};

				}
				else if(start_date.onOrAfter(change_start) && start_date.onOrBefore(change_end)){
					hasConflict = true;
					changeInfo = {
						"number": grGetChange.getDisplayValue("number"),
						"desc" : grGetChange.getDisplayValue("short_description")
					};

				}else if(end_date.onOrBefore(change_end) && end_date.onOrAfter(start_date)){
					hasConflict = true;
					changeInfo = {
						"number": grGetChange.getDisplayValue("number"),
						"desc" : grGetChange.getDisplayValue("short_description")
					};
				}


			}

			return  {"hasConflict": hasConflict, "change_info": changeInfo};
		}
	}


	data.calendarEvents = mFunctions.getEvents();

/*======== ======== ======== */
/* ======== HTML ======== */
/*======== ======== ======== */

  <!-- your widget template -->
  <div class="events-title">
    <h3>Calendário</h3>  
  </div>

  <div class="row">
    <div class="col col-md-9">
      <div id="calendar"></div>  
      <div>
        Legenda: 
        <i class="fa fa-circle" style="color:{{c.data.changesColorConflict}}" aria-hidden="true"></i> Change com conflito
        <i class="fa fa-circle" style="color:{{c.data.changesColor}}" aria-hidden="true"></i> Change sem conflito
        <i class="fa fa-circle" style="color:{{c.data.finnancialEventsColor}}" aria-hidden="true"></i> Eventos Financeiros
        <i class="fa fa-circle" style="color:{{c.data.tiEventsColor}}" aria-hidden="true"></i> Eventos de TI
        <i class="fa fa-circle" style="color:{{c.data.vacationsColor}}" aria-hidden="true"></i> Feriados
        <br>
        <small>observação: {{c.data.footerMessage}}</small>
      </div>
    </div>

    <div class="col col-md-3 col-sm-12">
      <div class="section-resume-events">
        <input ng-model="searchText" placeholder="Filtrar eventos"/>
        <div class="lupa-busca">
          <i class="fa fa-search"></i>
        </div>
        <div class="list-events">
          <ol ng-repeat="ev in c.data.calendarEvents | filter:searchText">
            <li ng-if="ev.type == 'event'">
              <small>{{ev.day}} de {{data.getMonth(ev)}}, {{ev.start_hour}}pm</small>
              <p class="typeEvent">{{ev.subtype}}</p> 
              <p>{{ev.title}}</p>
              <!-- <div class="feature-event">
                <div class="feature-day">
                  <p>{{ev.day}}</p>
                  <small>{{data.getMonth(ev)}} <br>{{ev.start_hour}} - {{ev.end_hour}} <br> {{ev.subtype}}</small> 
                </div>
                <div class="resume-event">
                  <p>
                    {{ev.title}}
                  </p>
                </div>
              </div> -->
            </li>
          </ol>  
        </div>

      </div>

    </div>
  </div>

</div>]]></template>
</sp_widget>
</unload>
