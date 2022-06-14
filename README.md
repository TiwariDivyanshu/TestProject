import React, { useState, useEffect } from 'react';
import Breadcrumbs from '@mui/material/Breadcrumbs';
import Link from '@mui/material/Link';
import Stack from '@mui/material/Stack';
import NavigateNextIcon from '@mui/icons-material/NavigateNext';
import { useNavigate, useLocation } from 'react-router-dom';
import { APP_ROOT, SRE_BREADCRUMBS } from '../../../utils/Constants';
import { GetChannelLevelPageTitle } from '../../../utils/utils';
import './BreadCrumbs.scss';
function handleClick(event) {
  event.preventDefault();
}
export default function BreadCrumbsNav() {
  const navigate = useNavigate();
  const location = useLocation();
  location.pathname = location.pathname.replace(APP_ROOT, '/');
  const [breadcrumbs, setbreadcrumbs] = useState([]);
  const title = GetChannelLevelPageTitle();
  let pageName = location?.pathname?.split('/')[3];
  const capitalizeFirst = str => {
    return str.charAt(0).toUpperCase() + str.slice(1);
  };
  useEffect(() => {
    let breadcrumbs = [];
    if (location?.pathname === '/') {
      breadcrumbs = [];
    }
    else if (location?.pathname?.split('/')[1] === 'flows' && location.state.prevPath === '/allflows') {
     breadcrumbs = [
        <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
          Dashboard
        </Link>,
        <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate(location.state.prevPath); }}>
          {capitalizeFirst(SRE_BREADCRUMBS["/" + location?.pathname?.split('/')[1]])}
        </Link>,
        <div className="active" key="1" color="inherit" >
          {capitalizeFirst(pageName)}
        </div>
      ];
    } else if (location?.pathname?.split('/')[1] === 'flows') {
      if (location.state.prevPath?.split('/')[1] === 'channel' || location.state.prevPath?.split('/')[1] === 'reports') {
        breadcrumbs = [
          <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
            Dashboard
          </Link>,
          <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate(location.state.prevPath); }}>
            {capitalizeFirst(location?.pathname?.split('/')[2].replace('and', '&').replaceAll('-', ' '))}
          </Link>,
          <div className="active" key="1" color="inherit" >
            {capitalizeFirst(pageName.replaceAll('-', ' '))}
          </div>
        ];
      }
      else {
        breadcrumbs = [
          <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
            Dashboard
          </Link>,
          <div className="active" key="1" color="inherit" >
            {capitalizeFirst(pageName.replaceAll('-', ' '))}
          </div>
        ];
      }
    }
    else if (location?.pathname?.split('/')[1] === 'reports') {
      if (location?.pathname?.split('/')[2] === 'alerts') {
        breadcrumbs = [
          <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
            Dashboard
          </Link>,
          <div className="active" key="1" color="inherit" >
            {capitalizeFirst(location?.pathname?.split('/')[2].replace('and', '&').replaceAll('-', ' ') + " " + location?.pathname?.split('/')[1].replace('and', '&').replaceAll('-', ' '))}
          </div>
        ];
      }
      else {
        breadcrumbs = [
          <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
            Dashboard
          </Link>,
          <div className="active" key="1" color="inherit" >
            {capitalizeFirst(location?.pathname?.split('/')[2].replace('and', '&').replaceAll('-', ' ')+ " " +'locations')}
          </div>
        ];
      }
    }
    else {
      breadcrumbs = [
        <Link underline="hover" className="pointer breadcrum-text" key="1" color="inherit" onClick={() => { navigate('/'); }}>
          Dashboard
        </Link>,
        <div className="active" key="1" color="inherit" >
          {capitalizeFirst(title.replaceAll('-', ' '))}
        </div>
      ];
    }
    setbreadcrumbs(breadcrumbs);
  }, [location]);
  return (
    <Stack spacing={1} className='breadcrumb-container'>
      <Breadcrumbs
        separator={<NavigateNextIcon fontSize="small" />}
        aria-label="breadcrumb"
      >
        {breadcrumbs}
      </Breadcrumbs>
    </Stack>
  );
}
